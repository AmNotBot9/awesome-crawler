# Entry Level Cybersecutiry Jobs

from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.chrome.service import Service
import time
import pandas as pd

# Path to your webdriver (modify with your path)
driver_path = '/path/to/chromedriver'  # Example for ChromeDriver

# Use Service object to specify the driver path
service = Service(driver_path)
driver = webdriver.Chrome(service=service)

# Define the Indeed URL for cybersecurity jobs
url = 'https://www.indeed.com/jobs?q=entry+level+cybersecurity&l=&start=0&vjk=72493d392f57ef16'

# Open the webpage
driver.get(url)

# Wait for the page to load
time.sleep(5)

# Find job postings (customize the XPath as per the site structure)
job_cards = driver.find_elements(By.CLASS_NAME, 'jobsearch-SerpJobCard')

# Lists to store data
job_titles = []
companies = []
locations = []

# Loop through job postings and extract information
for job_card in job_cards:
    try:
        # Extract job title
        title = job_card.find_element(By.CLASS_NAME, 'title').text
        job_titles.append(title)
        
        # Extract company name
        company = job_card.find_element(By.CLASS_NAME, 'company').text
        companies.append(company)

        # Extract location
        location = job_card.find_element(By.CLASS_NAME, 'location').text
        locations.append(location)
    
    except Exception as e:
        print(f"Error extracting data from job card: {e}")

# Close the browser
driver.quit()

# Create a DataFrame from the data
df = pd.DataFrame({
    'Job Title': job_titles,
    'Company': companies,
    'Location': locations
})

# Save the DataFrame to an Excel file
df.to_excel('indeed_jobs_cybersecurity.xls', index=False)
print("Data saved to 'indeed_jobs_cybersecurity.xls'")
