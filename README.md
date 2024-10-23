python
 
 import requests
from bs4 import BeautifulSoup

def scrape_jobs():
    url = 'https://www.indeed.com/jobs?q=software+developer'
    response = requests.get(url)
    soup = BeautifulSoup(response.text, 'html.parser')
    jobs = []
    
    for job in soup.find_all('div', class_='jobsearch-SerpJobCard'):
        title = job.find('h2').text
        link = job.find('a')['href']
        # Attempt to find the expiry date
        expiry_date = job.find('span', class_='expiry-date').text if job.find('span', class_='expiry-date') else 'No expiry date listed'
        
        jobs.append({
            'title': title,
            'link': link,
            'expiry_date': expiry_date
        })
    
    return jobs


HTML

<!-- job_display.html -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Job Listings</title>
</head>
<body>
    <h1>{{ job.title }}</h1>
    <p><a href="{{ job.link }}">View Job</a></p>
    <p>Expiry Date: {{ job.expiry_date }}</p>
    <form action="/apply" method="post">
        <input type="hidden" name="job_id" value="{{ job.id }}">
        <button type="submit">Apply</button>
    </form>
</body>
</html>


job_application_ai/
│
├── app.py
├── scraping.py
├── README.txt
└── job_display.html
