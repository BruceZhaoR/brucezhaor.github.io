1 show variables like 'collation_%';
2 show variables like 'character_set_%';

查看之后修改：

1 mysql> set character_set_client=utf8;
2 mysql> set character_set_connection=utf8;
3 mysql> set character_set_database=utf8;
4 mysql> set character_set_results=utf8;
5 mysql> set character_set_server=utf8;
6 mysql> set character_set_system=utf8;
7 mysql> set collation_connection=utf8;
8 mysql> set collation_database=utf8;
9 mysql> set collation_server=utf8;



set character_set_client=utf8;
set character_set_connection=utf8;
set character_set_results=utf8;


mysql -uroot -p

