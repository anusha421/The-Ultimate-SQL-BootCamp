-- find the 5 oldest users
select * from users order by created_at LIMIT 5;

-- what day of the week do most users register on
select dayname(created_at) as day from users 
group by day 
order by count(*) desc LIMIT 1;

-- find the users who have never posted a photo
select username from users 
left join photos on users.id = photos.user_id
 where photos.id is NULL;
 
 -- find the user with the most likes on a single photo
select username, photos.id, photos.image_url, count(*)
from photos
inner join likes 
on likes.photo_id = photos.id 
inner join users
on photos.user_id = users.id
group by photos.id order by count(*) desc LIMIT 1;

-- how many times does the average user post
select ((select count(*) from photos) / (select count(*) from users));

-- what are the top 5 most commonly used hashtags
select tag_name, count(*) from tags 
inner join photo_tags on
photo_tags.tag_id = tags.id
group by tags.id order by count(*) desc limit 5;

-- find users who have liked every single photo on the site
select username, count(likes.user_id) from users
inner join likes
on users.id = likes.user_id
join photos on
photos.id = likes.photo_id
group by likes.user_id having count(likes.user_id) =
(select count(*) from photos); 
