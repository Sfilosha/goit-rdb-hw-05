use mydb;

// 1st task
select od.\*,
(select o.customer_id
from orders o
where o.id = od.order_id) as customer_id
from order_details od

// 2nd task
select \*
from order_details
where order_id in
( select id
from orders
where shipper_id = 3 );

// 3rd task
select temp.order_id,
AVG(temp.quantity) as avg_quantity
from (select \* from order_details
where quantity > 10) as temp
group by temp.order_id;

//4th task
with details as (select \* from order_details where quantity > 10)
select
order_id,
AVG(quantity) as avg_quantity
from details
group by order_id
order by avg_quantity asc;

//5th task
drop function if exists divide;

DELIMITER //

create function divide(a float, b float)
returns float
deterministic

begin
return a / b;
end//

DELIMITER ;

select order_id, product_id, quantity,
divide(quantity, 4) as division_value
from order_details;
