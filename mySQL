use BD310718

drop table download.rswnt_pi.LOAN_NCBS_0718

SELECT A.*, B.InterestRate,B.AccPaymentAmount,B.AmountInterest,B.AmountPrincipal,C.CLKOLEK,C.CLKOLEKOVR

into download.rswnt_pi.LOAN_NCBS_0718
FROM
(
select 'NCBS' DataSource, 
a.COD_CUST_ID as CIF, 
g.COD_ACCT_CUST_REL as CustRelation,
a.COD_ACCT_NO as AccountNumber, 
--h.NAM_CUST_SHRT as NameAcct,
d.NAM_PRODUCT ProductName,
'LENDING' ProductGroup, 
d.COD_PROD as ProductCode,
case when a.COD_PROD in(800) then 'Personal Loan'
     when a.COD_PROD in(801,808,811,813) then 'Mortgage'
     when a.COD_PROD in(875) then 'Mortgage FCY'
     when a.COD_PROD in(802) then 'Back To Back'
     when a.COD_PROD in(805,82) then 'Multi Purpose'
     when a.COD_PROD in(803,942,946,961,962,965) then 'Car Loan'
     when a.COD_PROD in(804) then 'Motor Cycle Loan'
     when a.COD_PROD in(806,807) then 'Employee Loan'
     when a.COD_PROD in(809) then 'Personal Loan'
     when a.COD_PROD in(970,971) then 'Mortgage Syariah'	
else 'N/A' end Product,
j.txt_690 as MIS_Product,
c.COD_AO_BUSINESS as AcctOfficer, 
c.COD_AO_OPERATIONS as AcctOfficer2,
a.COD_CC_BRN as AcctBranchCode, 
e.NAM_BRANCH as AcctBranchName,
CONVERT(VARCHAR(10), h.DAT_ACCT_OPEN, 101) as AcctOpenDate,
a.COD_ACCT_STAT, 
CASE when a.COD_ACCT_STAT = 1 then 'CLOSED'
	 when a.COD_ACCT_STAT = 2 then 'BLOCKED'
	 when a.COD_ACCT_STAT = 3 then 'NODEBIT'
	 when a.COD_ACCT_STAT = 4 then 'NOCREDIT'
	 when a.COD_ACCT_STAT = 5 then 'CLOSED_TODAY'
	 when a.COD_ACCT_STAT = 6 then 'OPENED_TODAY'
	 when a.COD_ACCT_STAT = 7 then 'DORMANT'
	 when a.COD_ACCT_STAT = 8 then 'OPEN_REGULAR'
	 when a.COD_ACCT_STAT = 11 then 'WRITTEN_OFF'
	 when a.COD_ACCT_STAT = 2 then 'PAID_OFF 2'
	 when a.COD_ACCT_STAT = 20 then 'SETTLED_CHQ_PENDG'
	 when a.COD_ACCT_STAT = 24 then 'XFERED_OUT'
	 when a.COD_ACCT_STAT = 22 then 'PROD_XFERED_OUT'
else 'N/A' end AcctStatus,
CONVERT(VARCHAR(10), h.DAT_OF_MATURITY,101) as AcctNextMaturity,
h.CTR_TERM_MONTHS AS AcctTermMonth, 
H.COD_REMITTER_ACCT AS AcctDebet,

i.NAM_CCY_SHORT as AcctCurrency,
---j.rat_int as InterestRate,
h.AMT_FACE_VALUE as AcctPlafon,
--max j.amt_instal_outst AS AccPaymentAmount,
coalesce(SUM(a.AMT_PRINC_BALANCE*b.RAT_CCY_BOOK),0) IDRBalance,
coalesce(SUM(a.AMT_PRINC_BALANCE*b.RAT_CCY_BOOK),0) AvgBalance,
c.COD_LOB,
Lob= 
case when c.COD_LOB = 12 then 'PB' 
     when c.COD_LOB = 13 then 'AFFLUENT' 
     when c.cod_lob = 31 then 'SMEC'
else 'CMM' end


from 
BD_MIS_LN_ACCT_MAST  a left join
BD_BA_CCY_RATE  b on a.COD_CCY = b.COD_CCY left join
BD_BA_CUST_ACCT_AO_LOB_XREF  c on a.COD_ACCT_NO = c.COD_ACCT_NO left join
BD_LN_PROD_MAST  d on a.COD_PROD = d.COD_PROD left join
BD_BA_CC_BRN_MAST  e on a.COD_CC_BRN = e.COD_CC_BRN left join
BD_CH_ACCT_CUST_XREF  g on a.COD_ACCT_NO = g.COD_ACCT_NO left join
BD_MIS_LN_ACCT_DTLS  h on a.COD_ACCT_NO = h.COD_ACCT_NO left join
BD_BA_CCY_CODE  i on b.COD_CCY = i.COD_CCY left join
BD_UDF_ACCT_LOG_DETAILS_COLWISE_STG  j on a.COD_ACCT_NO = j.COD_ACCT_NO --,bd_mis_ln_acct_schedule_detls j--,BD_MIS_LN_ACCT_RATES K

where 
--(convert(char(2),b.DAT_TIM_RATE_EFF,3)=(substring(db_name(),3,2)+'/'+substring(db_name(),5,2)+ '/'+right(db_name(),2)) OR b.COD_CCY = 360)
--and 
a.COD_ACCT_STAT<>1
and c.COD_LOB in(12,13,31,10,11)
--and g.COD_ACCT_CUST_REL IN ('SOW','JOF','JAF','OWN')

group by
g.COD_ACCT_CUST_REL,a.COD_CUST_ID,
a.COD_ACCT_NO, a.COD_CCY, d.NAM_PRODUCT, d.COD_PROD, a.COD_CC_BRN, e.NAM_BRANCH, h.DAT_ACCT_OPEN,
a.COD_ACCT_STAT, c.COD_LOB, a.COD_PROD, c.COD_AO_BUSINESS,c.COD_AO_OPERATIONS, i.NAM_CCY_SHORT,h.DAT_OF_MATURITY,h.CTR_TERM_MONTHS,
h.AMT_FACE_VALUE,/*h.NAM_CUST_SHRT,*/H.COD_REMITTER_ACCT, j.txt_690--,j.rat_int,j.amt_instal_outst
) A LEFT JOIN
(
select cod_acct_no as Acct,amt_instal_outst as AccPaymentAmount,rat_int as InterestRate,amt_interest as AmountInterest,
amt_principal as AmountPrincipal--max (amt_instal_outst) as AccPaymentAmount,rat_int as InterestRate
from  bd_mis_ln_acct_schedule_detls (NOLOCK)
---Ubah Tanggal
where month(date_instal)=7 and year(date_instal)=2018
---group by cod_acct_no,rat_int
)B ON A.ACCOUNTNUMBER=B.ACCT LEFT JOIN
(
select CLNOREK,CLKOLEK,CLKOLEKOVR
from dbo.NCBS_CL (NOLOCK)
)C ON A.ACCOUNTNUMBER=C.CLNOREK





