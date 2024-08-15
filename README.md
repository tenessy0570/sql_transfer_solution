1) Первый способ - приджойнить таблицу с мЕньшим в таблицу с бОльшим количеством и из полученного результата создать новую таблицу:
```shell
Create table full_names_with_statuses as (
  select s.id, f.name, s.status from full_names as f
	left outer join short_names as s on f.name LIKE (s.name || '.%')
)
```

2) Второй способ - просто апдейтить поле status у каждой записи в таблице full_names
```shell
DO
$$
DECLARE short_name_row RECORD;
BEGIN
    FOR short_name_row IN 
      SELECT * from short_names
    LOOP
      UPDATE full_names
      SET status = short_name_row.status
      WHERE name LIKE (short_name_row.name || '.%');
    END LOOP;
END
$$;
```