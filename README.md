# Tech-job-portal-by-scraping-indeed-and-glassdoor
import requests
from bs4 import BeautifulSoup
import pandas as pd
def scrape_indeed_jobs(search_query, location,
num_pages=1):
"""Scrape job postings from Indeed."""
job_listings = []
base_url =
"https://www.indeed.com/jobs"
for page in range(num_pages):
params = {
'q': search_query, # Job title or keyword
'l': location, # Job location
'start': page * 10 # Pagination
}
headers = {
'User-Agent': 'Mozilla/5.0 (Windows NT 10.0;
Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko)
Chrome/91.0.4472.124 Safari/537.36'
}
response = requests.get(base_url, params=params,
headers=headers)
if response.status_code != 200:
print(f"Failed to fetch Indeed page {page + 1}. Status code: {response.status_code}")
continue
soup = BeautifulSoup(response.text,
'html.parser')
jobs = soup.find_all('div'
,
class_=
'job_seen_beacon') # Update based on
Indeed's current structure
for job in jobs:

title = job.find('h2'
, class_=
'jobTitle').text.strip()
if job.find('h2'
, class_=
'jobTitle') else 'Not listed'
company = job.find('span'
,
class_=
'companyName').text.strip() if job.find('span'
,
class_=
'companyName') else 'Not listed'
location = job.find('div'
,
class_=
'companyLocation').text.strip() if job.find('div'
,
class_=
'companyLocation') else 'Not listed'
summary = job.find('div'
, class_=
'job- snippet').text.strip() if job.find('div'
, class_=
'job- snippet') else 'Not listed'
job_listings.append({
'Job Title': title,
'Company': company,
'Location': location,
'Summary': summary,
'Source': 'Indeed'
})
return pd.DataFrame(job_listings)
def scrape_glassdoor_mock_data():
"""Simulate Glassdoor scraping with mock data."""
mock_data = [
{
'Job Title': 'Software Engineer'
,
'Company': 'Tech Innovators'
,
'Location': 'San Francisco, CA'
,
'Summary': 'Develop and maintain cutting-edge
software solutions.'
,
'Source': 'Glassdoor'
},
{
'Job Title': 'Data Scientist'
,
'Company': 'AI Analytics'
,
'Location': 'New York, NY'
,
'Summary': 'Analyze data to drive business
decisions.'
,
'Source': 'Glassdoor'
}

]
return pd.DataFrame(mock_data)
def combine_job_dataframes(indeed_df, glassdoor_df):
"""Combine job listings from both sources into one
DataFrame."""
return pd.concat([indeed_df, glassdoor_df],
ignore_index=True)
# Example usage
if _name_ ==
"_main_":
search_query =
"Software Engineer"
location =
"San Francisco, CA"
# Scrape Indeed jobs (first 2 pages)
indeed_jobs = scrape_indeed_jobs(search_query,
location, num_pages=2)
print("Indeed Jobs:")
print(indeed_jobs)
# Simulate Glassdoor jobs
glassdoor_jobs = scrape_glassdoor_mock_data()
print("\nGlassdoor Mock Jobs:")
print(glassdoor_jobs)
# Combine both data sources
all_jobs = combine_job_dataframes(indeed_jobs,
glassdoor_jobs)
print("\nCombined Job Listings:")
print(all_jobs)
# Save to CSV
all_jobs.to_csv("combined_job_listings.csv"
,
index=False)
