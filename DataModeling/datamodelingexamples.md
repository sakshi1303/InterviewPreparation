## Movie Booking System

<details>
<summary>Answer</summary>

```sql
create table movie(
movie_id number,
movie_name varchar2(255),
movie_language varchar2(100));

create table auditorium(
auditorium_id number,
theatre_id number);

create table theatre(
theatre_id number,
theatre_name varchar2(100));

create table seats(
seat_id number,
row_number number,
column_no varchar2(1),
auditorium_id number);

create table screening(
screening_id number,
auditorium_id number,
movie_id number,
starttime date);

create table reservation(
reservation_id number,
customer_id number,
payment_type number 
);

create table seat_booking(
seat_id number,
screening_id number,
reservation_id number);
```
</details>

## IPL Data Model

<details>
<summary>Answer</summary>

```sql
batsman
batsman_id
batsman_name

match
match_id
match_date
match_time

ipl
batsman_id
match_id
runs_scored
year
```

</details>

## Hotel Reservation Data Model

<details>
<summary>Answer</summary>

```sql
create table hotel (hotel_id number);

create table seats (seatid number, seattype number);

create table booking (booking_id number, hotel_id number, seat_id number, booking_date date, booking_time varchar2(20));
```

</details>

## Orders Data Model

<details>
<summary>Answer</summary>

```sql
create table orders(
order_id number,
customer_id number,
order_date date,
order_time varchar(20),
order_amount number);

create table order_line(
order_id number,
order_line_id number,
product_id number,
delivery_address_id number);

create table deliver_address(
delivery_address_id number,
address_line_1 varchar2(50),
address_line_2 varchar2(50),
address_line_3 varchar2(50),
pin number,
state varchar2(50));
```

</details>

## Song Reviews Data Model

<details>
<summary>Answer</summary>

```sql
create table song_dim (
song_id number,
song_name varchar2(255));

create table views_fact(
song_id number,
views number,
eff_dt date)

create table date_dim(
eff_dt date,
eff_year number
);
```

</details>

## Movie Reviewer Data Model

<details>
<summary>Answer</summary>

```sql
create table movie 
(
movie_id number,
movie_name varchar2(255)
);

create table reviewer
(
reviewer_id number,
reviewer_name varchar2(255)
);

create table rating
(
rating_id number,
movie_id number,
reviewer_id number,
rating_date date,
ratings number
);
```
</details>
