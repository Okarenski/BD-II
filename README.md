# BD-II
Aulas de BD-II (Saldanha)

tabela no from (quero todos) 
a segunda tabela (left join)

*crude
-create = insert
-read = selet
-update = update
-delete = delete

where / order / truncate / droptable

New = recebe o novo (insert e update)
old = recebe o antigo (insert e update)
TG_NAME (nome da trigger)
TG_WHEN ( momento da execução 'BEFORE' ou 'AFTER'
TG_LEVEL (nivel do  trigger 'ROW' ou 'STATEMENT'
TG_OP (operação que disparou 'INSERT' , 'UPDATE', 'DELETE' ou 'TRUNCADE'
TG_RELID (OID da tabela que disparou o trigger)
TG_TABLE_NAME (nome da tabela)
TG_TABLE_SCHEMA ( nome do schema onde esta a tabela)
TG_ARGV [] text [] (array com argumentos passados para o trigger na criação (CREATE TRIGGER... EXECUTE FUNCTION ... (arg1, arg2,...)
