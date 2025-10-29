from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
import time

def scrape_google(query):
    driver = webdriver.Chrome()
    driver.get("https://www.google.com")

    # search
    search_box = driver.find_element(By.NAME, "q")
    search_box.send_keys(query)
    search_box.send_keys(Keys.RETURN)
    time.sleep(3)

    # scroll for more results
    for _ in range(3):
        driver.find_element(By.TAG_NAME, "body").send_keys(Keys.END)
        time.sleep(2)

    # collect results
    results_data = []
    results = driver.find_elements(By.CSS_SELECTOR, "div.MjjYud")

    for result in results:
        try:
            title = result.find_element(By.TAG_NAME, "h3").text
            link = result.find_element(By.TAG_NAME, "a").get_attribute("href")
            results_data.append((title, link))
        except:
            pass

    input("Press Enter to close browser...")
    driver.quit()
    return results_data


if _name_ == "_main_":
    query = input("Enter search query: ")
    data = scrape_google(query)

    for title, link in data:
        print(f"TITLE: {title}")
        print(f"LINK: {link}")
        print("-" * 50)
