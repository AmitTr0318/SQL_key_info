# SQL_key_info
**Contains sample sql queries, stored_procedures for reference**

**Stored_Procedure Query**
CREATE DEFINER=`root`@`localhost` PROCEDURE `get_monthly_gross_sales_for_customer`(in_c_code text)
BEGIN
SELECT 
    s.date,
    sum(gross_price) as total_sale
  
FROM
    fact_sales_monthly s
        JOIN
    dim_product p ON s.product_code = p.product_code
        JOIN
    fact_gross_price g ON s.product_code = g.product_code
        AND g.fiscal_year = GET_FISCAL_YEAR(s.date)
where find_in_set(s.customer_code, in_c_code)
group by s.date;

END

**User defined function Query**
CREATE DEFINER=`root`@`localhost` FUNCTION `get_fiscal_qtr`(
calendar_date date) RETURNS char(2) CHARSET utf8mb4
    DETERMINISTIC
BEGIN
	declare m tinyint;
    declare qtr char(2);
    set m = month(calendar_date);
    case
		when m in (9,10,11) then set qtr = "Q1";
		when m in (12,1,2) then set qtr = "Q2";
        when m in (3,4,5) then set qtr ="Q3";
        else set qtr ="Q4";
    end case;
RETURN qtr;
END
