# Responsi  Junior Project
|Nama | NIM|
| --- | --- |
| Muhammad Rendy | 22/503275/TK/54980 |

## Entity Relationship Diagram for Departement and Employee Database
![](erd.png)

## Setting Up Database
```sql
create sequence dep_id start 1;
create table departemen(
	id_dep character varying primary key,
	nama_dep character varying (30)
)
```
``` sql
create sequence users_id start 1;
create table karyawan(
	id_karyawan character(6) default 'K'||nextval('users_id') primary key,
	nama varchar(30),
	id_dep character varying references departemen(id_dep)
);
```

``` sql
create or replace function kr_insert
(
	_nama character varying,
	_id_dep character varying
)

returns int AS
'
begin
	insert into public.karyawan
	(
		nama,
		id_dep
	)
	
	values
	(
		_nama,
		_id_dep
	);
	if found then
		return 1;
	else
		return 0;
	end if;
end
'
language plpgsql
```

``` sql
create or replace function kr_select()
returns table
(
	_id_karyawan character,
	_nama character varying,
	_id_dep character varying,
	_nama_dep character varying
)

language plpgsql
as
'
begin
	return query
	select karyawan.id_karyawan, karyawan.nama, departemen.id_dep, departemen.nama_dep FROM departemen JOIN karyawan ON karyawan.id_dep = departemen.id_dep;
end
'
```

``` sql
create or replace function kr_update
(
	_id_karyawan character,
	_nama character varying,
	_id_dep character varying
)

RETURNS int AS

'
begin
	update karyawan SET 
		nama = _nama,
		id_dep = _id_dep
	WHERE id_karyawan = _id_karyawan;
	if found then
		return 1;
	else
		return 0;
	end if;
end
'
language plpgsql
```

``` sql
create or replace function kr_delete (_id_karyawan character varying)
RETURNS int AS

'
begin
	delete from public.karyawan
	WHERE id_karyawan=_id_karyawan;
	if found then
		return 1;
	else
		return 0;
	end if;
end
'
language plpgsql
```

``` sql
insert into departemen(id_dep, nama_dep) values
('HR', 'HR'),
('ENG', 'Engineer'),
('DEV', 'Developer'),
('PM', 'Product Manager'),
('FIN', 'Finance');
```
