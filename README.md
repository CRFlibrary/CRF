# Database
DBImport
run run DB.env

--TO export tables by table ames using EXP utility in EC2/local Oracle DB instance

exp GL@CWEBSV01 FILE=exp_file.dmp TABLES=GL_BALANCES,GL_BUDGETS,GL_BUDGET_TYPES,GL_BUDGET_VERSIONS,GL_CODE_COMBINATIONS,GL_DAILY_BALANCES,GL_DAILY_RATES,GL_DATE_PERIOD_MAP,GL_JE_BATCHES,GL_JE_HEADERS,GL_JE_LINES,GL_LEDGERS LOG=exp_file.log

exp APPLSYS@CWEBSV01 FILE=FND_VALUES.dmp TABLES=FND_FLEX_VALUES,FND_FLEX_VALUES_TL  LOG=FND_VALUES.log

sqlplus 'crfdb_user@(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=orcl.c7y14itdrmil.eu-west-1.rds.amazonaws.com)(PORT=1521))(CONNECT_DATA=(SID=ORCL)))'     



-- Log 


. . exporting table                    GL_BALANCES    8232309 rows exported
. . exporting table                     GL_BUDGETS        220 rows exported
. . exporting table                GL_BUDGET_TYPES          1 rows exported
. . exporting table             GL_BUDGET_VERSIONS        220 rows exported
. . exporting table           GL_CODE_COMBINATIONS     100219 rows exported
. . exporting table              GL_DAILY_BALANCES      61348 rows exported
. . exporting table                 GL_DAILY_RATES    1187249 rows exported
. . exporting table             GL_DATE_PERIOD_MAP     195050 rows exported
. . exporting table                  GL_JE_BATCHES      86525 rows exported
. . exporting table                  GL_JE_HEADERS     177917 rows exported
. . exporting table                    GL_JE_LINES    3155216 rows exported
. . exporting table                     GL_LEDGERS         98 rows exported

--TO Import tables by table ames using IMP utility in EC2/local Oracle DB instance to RDS as target


---CONFIGURE TNS ENTRY FOR RDS IN EC2 (Source Instance) IMP Utility has to be run in Source environment


imp crfdb_user@DB_RDS FROMUSER=GL TOUSER=GL  TABLES=GL_BALANCES,GL_BUDGETS,GL_BUDGET_TYPES,GL_BUDGET_VERSIONS,GL_CODE_COMBINATIONS,GL_DAILY_BALANCES,GL_DAILY_RATES,GL_DATE_PERIOD_MAP,GL_JE_BATCHES,GL_JE_HEADERS,GL_JE_LINES,GL_LEDGERS FILE=exp_file.dmp LOG=imp_file.log

imp crfdb_user@DB_RDS FROMUSER=GL TOUSER=GL TABLES=GL_CODE_COMBINATIONS FILE=exp_file.dmp GRANTS=no LOG=imp_file.log


imp crfdb_user@DB_RDS FROMUSER=GL TOUSER=GL TABLES=GL_CODE_COMBINATIONS FILE=exp_file.dmp GRANTS=no LOG=imp_file.log


imp crfdb_user@DB_RDS FROMUSER=APPLSYS TOUSER=GL TABLES=FND_FLEX_VALUES,FND_FLEX_VALUES_TL  FILE=FND_VALUES.dmp GRANTS=no LOG=imp_file.log
