
# SCRAPE-HISTORY
Database dump of historical scrapes from production (makes test database):
- test_scrape_correct (chosen correct business)
- test_scrape_geo (user selected by geo map. Address is accurate)
- test_scrape_manual (user entered address)
_ test_scrape_matches #1 and #2 (scrapes done for businesses)

Due to github 100Mb limit the scrape_manual table is split into two files.

Query for scrape matches:
DROP TABLE scrape_matches;
CREATE TABLE scrape_matches
SELECT campaign_id, w.name as website, street, city, region, postal_code, phone, business_name, 
       is_default, desktop_url
FROM  `campaign_info` 
join websites as w on w.id = website_id
WHERE  
       website_id IN ( 1, 3, 4, 7, 12, 14, 20, 22, 112, 115, 118, 128, 129 ) 
       and business_name <> ‘’  


Query to create table of manual businesses (user entered address):
DROP TABLE scrape_manual; create table scrape_manual 
SELECT campaign_id, street, city, region, postal_code, phone, business_name, 
       is_default, desktop_url 
FROM `campaign_info` 
WHERE website_id is null  and lat is null and ( is_default = 0 or (is_default = 1 and campaign_id < 7722)) and business_name <> ''

Query to create table of geo-selected businesses (selected from map):
DROP TABLE scrape_geo; create table scrape_geo 
SELECT campaign_id, street, city, region, postal_code, phone, business_name,lat, lng, 
       is_default, desktop_url 
FROM `campaign_info` 
WHERE website_id is null  and lat is not null and (is_default = 0 or (is_default = 1 and campaign_id < 7722)) and business_name <> ''
