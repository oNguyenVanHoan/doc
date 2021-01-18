1. Update multi row

```SQL
update public.users as row set
    first_name = data.first_name,
    last_name = data.last_name,
    detail = data.detail
from (values
    (1, 'A', 'B', '{"locales":"HN"}'::jsonb),
    (2, 'C', 'D', '{"locales":"HN"}'::jsonb),
    (3, 'E', 'F', '{"locales":"HN"}'::jsonb)
) as data(id, first_name, last_name, detail)
where data.id = row.id
```

```jsvascript
let updateData = '';
payload.forEach(el => {
    updateData += `('${el.id}','${JSON.stringify(el.detail)}'::jsonb),`;
});
if(updateData.length > 0){
    return knex.raw(`
        update public.ad_accounts as row set
        detail = data.detail
        from (values ${updateData.slice(0, -1)})
        as data(id, detail)
        where data.id = row.id
    `);
}
```

2. Search without accent
Note: Need permission
```SQL
CREATE EXTENSION IF NOT EXISTS unaccent;

SELECT *
FROM "campaigns"
WHERE  unaccent(facebook_name) ILIKE unaccent('Chien dịch moi');
```

3. Truncate table
```
truncate ad_accounts RESTART IDENTITY CASCADE
```