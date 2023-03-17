_______________________________________________________________________________________________________________________________________________________
USE ig_clone;

We want to reward the user who has been around the longest, Find the 5 oldest users
select username, created_at from users 
order by created_at limit 5;

 To understand when to run the ad campaign, figure out the day of the week most users register on?

select dayname(created_at) as day_most_user_register,
count(created_at) as no_of_user_register 
from users 
group by  day_most_user_register 
order by no_of_user_register desc limit 2;

To target inactive users in an email ad campaign, find the users who have never posted a photo 

select users.id, users.username, photos.id from users 
left join photos on photos.user_id = users.id
where photos.id is null;


Suppose you are running a contest to find out who got the most likes on a photo. Find out who won? 

select  photos.id, users.username, count(photo_id) as most_liked_photo from likes
join photos on photos.id = likes.photo_id
join users on users.id = likes.user_id
group by photos.id
order by most_liked_photo 
desc limit 1;


The investors want to know how many times does the average user post. 

SELECT round((SELECT COUNT(id)FROM photos)/(SELECT COUNT(id) FROM users),2) as avg_user_post;

 A brand wants to know which hashtag to use on a post, and find the top 5 most used hashtags 

select tags.tag_name, count(tag_name) as most_used_tags
from tags
join photo_tags on tags.id = photo_tags.tag_id
group by tags.id
order by most_used_tags desc limit 5;


To find out if there are bots, find users who have liked every single photo on the site 

select users.id, users.username, count(users.id) As total_likes_by_user
from users
join likes on users.id = likes.user_id
group by users.id
having total_likes_by_user = (select COUNT(image_url) from photos);

 To know who the celebrities are, find users who have never commented on a photo 

select users.id, users.username from users 
left join comments on users.id = comments.user_id 
group by comments.user_id
order by comments.id is null;


 Now it's time to find both of them together, find the users who have never commented on any photo or have commented on every photo 

select users.id, users.username, comments.comment_text, 
count(comment_text) as total_comment_count from users
left join comments on users.id = comments.user_id 
group by users.id
having comment_text is null or (count(comment_text) )= (select COUNT(image_url) from photos)
order by total_comment_count desc;


________________________________________________________________________________________________________________________________________________________


