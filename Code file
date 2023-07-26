declare
cursor dict is select cl.constraint_name, cl.TABle_name, cl.column_name from user_cons_columns cl,
user_constraints cn
where cn.constraint_name = cl.constraint_name and cl.column_name in (select column_name from
user_tab_columns
where data_type = 'NUMBER')and cn.constraint_type = 'P' and cl.TAble_name <> 'JOB_HISTORY';
-- declaring the cursor which is looping arrount table names and primary columns

tablname varchar2(100);
pri_col varchar2(100);
mx_vl number(8,2);
seq_name varchar2(100);
trig_name varchar2(100);
sec_max number(8,2);
inc_by number(8,2);
--declaring variable which are used into the creation of the sequence and the trigger
begin

for c in dict loop

tablname := c.TABLE_NAME;
pri_col := c.COLUMN_NAME;
seq_name := 'PK_SEQ_'||tablname;
trig_name :='PK_TRIG_'||tablname;
--opening the cursor and assiging the table name and the primary column name to variables


for seq1_rec in ( select * from user_sequences where sequence_name like '%'||tablname||'%') loop
execute immediate'drop sequence '||seq1_rec.sequence_name;
end loop;
-- checking and dropping old sequences with table names

for seq2_rec in (select * from user_sequences where  sequence_name like '%'||pri_col||'%' ) loop
execute immediate'drop sequence '||seq2_rec.sequence_name;
end loop;

-- checking and dropping  old sequences  with columns names


execute immediate 'select max('||pri_col||') from '||tablname
into mx_vl;
--calcucating mx_vl var which is the value that we shoud use it in the sequence as a start value

execute immediate 'select '||pri_col||' from (select '||pri_col||' from '||tablname||' order by
'||pri_col||' desc) where rownum = 1 and '||pri_col||' not in (select max('||pri_col||') from
'||tablname||' ) '
into sec_max;
--calculating the value of increment by difference between the max val and the second max val in the primary column and assigning it to inc_by variable;
inc_by := mx_vl - sec_max;

if mx_vl is null then
mx_vl := 1;
inc_by := 1;
else
mx_vl := mx_vl + inc_by;
end if;

--resovling the problem of the null values
execute immediate ' CREATE SEQUENCE '||seq_name||' start with '|| mx_vl ||' INCREMENT BY '
||inc_by||'
MAXVALUE 999999999999999999999999999
';
-- creating the sequance ...
----------------------------------------
execute immediate ' CREATE OR REPLACE TRIGGER ' ||trig_name||
' BEFORE INSERT
ON '||tablname||'
REFERENCING NEW AS New OLD AS Old
FOR EACH ROW
BEGIN
:new.'||pri_col||' := '|| seq_name||'.nextval;
END;
';
--creating the trigger and then we're done ! :))
end loop;
end;
