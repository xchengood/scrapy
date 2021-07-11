# scrapy
# Overview

**This repository store a basic example to learn scrapy better, which can simply and quickly capture population data of 235 countries over 60 years**

# Requirements

- Python: 3.8.8
- Scrapy: 2.5.0
- System: Mac OS X 10.15.7


# Install 

```
$ git clone git@github.com:xchengood/scrapy.git
```

# Usage

You can use the scrapy simply, just to do below:

```
$ cd worldometers
$ scrapy crwal countries
```

**If you want to store the result into the format of json/html/csv, just to do :**

```
$ scrapy crwal countries -o filename.json
($ scrapy crwal countries -o filename.html)
($ scrapy crwal countries -o filename.csv)
```

### Advanced Usage

* Run `scrapy startproject countries` to start a new project.  
  It will automatically generate most things, the only left things are:
  * `PROJECT/items.py`
  * `PROJECT/spider/countries.py`
  * `PROJECT/middlewares.py`
  * `PROJECT/pipelines.py`
  * `PROJECT/settings.py`

#### Example to hack  `spider.py`

Hacked `spider.py` with start rules and css rules (here only display the class exampleSpider):  

```
class CountriesSpider(scrapy.Spider):
    name = 'countries'
    allowed_domains = ['www.worldometers.info']
    start_urls = ['https://www.worldometers.info/world-population/population-by-country/']

    
    def parse(self, response):
        
        # countries = response.xpath("//td/a")
        # for country in countries:
        #     name = country.xpath(".//text()").get()
        #     link = country.xpath(".//@href").get()

        #     #absolute_url = f"https://www.worldometers.info{link}"
        #     #absolute_url = response.urljoin(link)
        #     #yield scrapy.Request(url=absolute_url)
        yield response.follow(url="https://www.worldometers.info/world-population/china-population/", callback=self.parse_country,meta={'country_name':"China"})

    def parse_country(self,response):
        #open_in_browser(response)
        #inspect_response(response, self)
        #logging.warning(response.status)
        name = response.request.meta['country_name']
        rows = response.xpath("(//table[@class='table table-striped table-bordered table-hover table-condensed table-list'])[1]/tbody/tr")
        for row in rows:
            year = row.xpath(".//td[1]/text()").get()
            population = row.xpath(".//td[2]/strong/text()").get()
            yield {
                'country_name': name,
                'year': year,
                'population': population
           }
```

# Example data with json and html format

![Screen Shot 2021-07-10 at 10.51.05 PM](https://github.com/xchengood/myPicture/blob/main/Screen%20Shot%202021-07-10%20at%2010.51.25%20PM.png)

![Screen Shot 2021-07-10 at 10.51.25 PM](https://github.com/xchengood/myPicture/blob/main/Screen%20Shot%202021-07-10%20at%2010.51.05%20PM.png)

# License
MIT License

Copyright (c) 2021 Xin Chen

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.



