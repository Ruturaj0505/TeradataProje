1. address {door_no:101, location: xxx, pin:1234,  }

2. write a query to join orders and customer table to get list of customers orders the product from 2026-Oct

select 
c.name
c.cutomer_id
o.order_id
from customer c
left join order o
on c.customer_id=o.order_id
where order_date>adddate(month,-1,2026)

3.what is diff between store producre and functions
4.write qery to find current and start location of employee
em_id  name    start_city curreent_city   year
1      Rahul    Mumbai     Pune           2022
1      Rahul    Mumbai     delhi            2024

5. 
left join tell me how many left join values
