---
layout: post
title:      "Movie Industry Analysis: Web Scraping "
date:       2020-07-31 21:09:57 -0400
permalink:  blog_number_2_by_hiten_patel
---


**Introduction**


“Microsoft sees all the big companies creating original video content, and they want to get in on the fun. They have decided to create a new movie studio, but the problem is they don’t know anything about creating movies. They have hired you to help them better understand the movie industry. Your team is charged with doing data analysis and creating a presentation that explores what type of films are currently doing the best at the box office. You must then translate those findings into actionable insights that the CEO can use when deciding what type of films they should be creating.”

**Focus**

There are hundreds of factors that can contribute to the success of a movie. These factors include production company, director, cast, runtime of the movie, the genre, the script, and time of release. In addition, there is also viewer and critic ratings that can impact the success of a movie as well. 

To help analysis our problem; we will focus on the following questions; 

* Which director(s) should we hire?
* Which genres are the most popular?
* Which ratings are the most popular ?

To help answer these questions, we will use IMDB Top 250 movies list as our data source. We will retrieve the necessary information like movie title, release date, director, genre, rating, and IMDB score using the web scraping method. 

**Web Scraping**

We will be using the "requests" package and "BeautifulSoup" to retrieve information from our desired website which is IMDB Top 250. 

```
import requests
url = 'https://www.imdb.com/search/title/?groups=top_250&sort=user_rating'
response = requests.get(url)
response.status_code


from bs4 import BeautifulSoup 
soup = BeautifulSoup(response.content, 'html.parser')
```

Next, we right-click anywhere on the website and select "Inspect". This pops up a window to the right that displays the html for that current webpage. From there, you navigate through the Document Object Model (DOM)  which provides an interface for programs to change the structure, style, and content of web pages. Beautiful Soup is a Python library designed for quick scraping projects. It allows you to select and navigate the tree-like structure of HTML documents, searching for particular tags, attributes or ids. 

![](https://raw.githubusercontent.com/learn-co-students/dsc-web-scraping-with-beautiful-soup-onl01-dtsc-ft-070620/master/images/DOM-model.svg.png)

First, we are going to retrieve the titles of all the movies from our IMDB Top 250. 
```

title_content = soup.find('div', class_='lister-list')
list_of_h3= title_content.find_all('h3')

final_title = []
for h3 in list_of_h3: 
    text = h3.text.replace('\n', '').split('.')[1]
    text = text[:-6]
    final_title.append(text) 
print (final_title)
```

```
['The Shawshank Redemption', 'The Godfather', 'The Dark Knight', 'The Godfather: Part II', 'The Lord of the Rings: The Return of the King', 'Pulp Fiction', "Schindler's List", '12 Angry Men', 'Hamilton', 'Inception', 'Fight Club', 'The Lord of the Rings: The Fellowship of the Ring', 'Forrest Gump', 'The Good, the Bad and the Ugly', 'The Lord of the Rings: The Two Towers', 'The Matrix', 'Goodfellas', 'Star Wars: Episode V - The Empire Strikes Back', "One Flew Over the Cuckoo's Nest", 'Harakiri', 'Parasite', 'Interstellar', 'City of God', 'Spirited Away', 'Saving Private Ryan', 'The Green Mile', 'Life Is Beautiful', 'Se7en', 'The Silence of the Lambs', 'Star Wars: Episode IV - A New Hope', 'Seven Samurai', "It's a Wonderful Life", 'Joker', 'Whiplash', 'The Intouchables', 'The Prestige', 'The Departed', 'The Pianist', 'Gladiator', 'American History X', 'The Usual Suspects', 'Léon: The Professional', 'The Lion King', 'Terminator 2: Judgment Day', 'Cinema Paradiso', 'Grave of the Fireflies', 'Back to the Future', 'Anand', 'Once Upon a Time in the West', 'Psycho']
```

Perfect. Next, let's retrieve the release year, movie genre, movie certification, IMDB rating, and the director for each movie. 

```

title_content = soup.find('div', class_='lister-list')
list_of_h3= title_content.find_all('h3')

release_year = []
for h3 in list_of_h3:
    text = h3.text[-6:-2]
    release_year.append(text)
print(release_year)
```
```

['1994', '1972', '2008', '1974', '2003', '1994', '1993', '1957', '2020', '2010', '1999', '2001', '1994', '1966', '2002', '1999', '1990', '1980', '1975', '1962', '2019', '2014', '2002', '2001', '1998', '1999', '1997', '1995', '1991', '1977', '1954', '1946', '2019', '2014', '2011', '2006', '2006', '2002', '2000', '1998', '1995', '1994', '1994', '1991', '1988', '1988', '1985', '1971', '1968', '1960']
```

```

title_content = soup.find('div', class_='lister-list')
genre = title_content.find_all('span', class_= 'genre')

final_genre = []
for g in genre:
    words = g.text.strip()
    final_genre.append(words)
print(final_genre)
```
```
['Drama', 'Crime, Drama', 'Action, Crime, Drama', 'Crime, Drama', 'Action, Adventure, Drama', 'Crime, Drama', 'Biography, Drama, History', 'Crime, Drama', 'Biography, Drama, History', 'Action, Adventure, Sci-Fi', 'Drama', 'Action, Adventure, Drama', 'Drama, Romance', 'Western', 'Action, Adventure, Drama', 'Action, Sci-Fi', 'Biography, Crime, Drama', 'Action, Adventure, Fantasy', 'Drama', 'Action, Drama, Mystery', 'Comedy, Drama, Thriller', 'Adventure, Drama, Sci-Fi', 'Crime, Drama', 'Animation, Adventure, Family', 'Drama, War', 'Crime, Drama, Fantasy', 'Comedy, Drama, Romance', 'Crime, Drama, Mystery', 'Crime, Drama, Thriller', 'Action, Adventure, Fantasy', 'Action, Adventure, Drama', 'Drama, Family, Fantasy', 'Crime, Drama, Thriller', 'Drama, Music', 'Biography, Comedy, Drama', 'Drama, Mystery, Sci-Fi', 'Crime, Drama, Thriller', 'Biography, Drama, Music', 'Action, Adventure, Drama', 'Drama', 'Crime, Mystery, Thriller', 'Action, Crime, Drama', 'Animation, Adventure, Drama', 'Action, Sci-Fi', 'Drama', 'Animation, Drama, War', 'Adventure, Comedy, Sci-Fi', 'Drama, Musical', 'Western', 'Horror, Mystery, Thriller']
```

```
title_content = soup.find('div', class_='lister-list')
rating = title_content.find_all('span', class_= 'certificate')

certification = []
for p in rating:
    new_rating = p.text
    certification.append(new_rating)
    
print (certification)
```
```
['R', 'R', 'PG-13', 'R', 'PG-13', 'R', 'R', 'Approved', 'PG-13', 'PG-13', 'R', 'PG-13', 'PG-13', 'R', 'PG-13', 'R', 'R', 'PG', 'R', 'Not Rated', 'R', 'PG-13', 'R', 'PG', 'R', 'R', 'PG-13', 'R', 'R', 'PG', 'Not Rated', 'PG', 'R', 'R', 'R', 'PG-13', 'R', 'R', 'R', 'R', 'R', 'R', 'G', 'R', 'R', 'Not Rated', 'PG', 'Not Rated', 'PG-13', 'R']
```

```

title_content = soup.find('div', class_='lister-list')
imdb = title_content.find_all('div', class_='inline-block ratings-imdb-rating')

imdb_rating = []
for rate in imdb:
    rating = rate.text.strip()
    imdb_rating.append(rating)
print (imdb_rating)
```
```
['9.3', '9.2', '9.0', '9.0', '8.9', '8.9', '8.9', '8.9', '8.8', '8.8', '8.8', '8.8', '8.8', '8.8', '8.7', '8.7', '8.7', '8.7', '8.7', '8.7', '8.6', '8.6', '8.6', '8.6', '8.6', '8.6', '8.6', '8.6', '8.6', '8.6', '8.6', '8.6', '8.5', '8.5', '8.5', '8.5', '8.5', '8.5', '8.5', '8.5', '8.5', '8.5', '8.5', '8.5', '8.5', '8.5', '8.5', '8.5', '8.5', '8.5']
```

```
title_content = soup.find('div', class_='lister-list')
director = title_content.find_all('p', class_="")
#director[0].find('a').text

final_directors = []
for person in director:
    final = person.find('a').text
    final_directors.append(final)
print(final_directors)
```
```

['Frank Darabont', 'Francis Ford Coppola', 'Christopher Nolan', 'Francis Ford Coppola', 'Peter Jackson', 'Quentin Tarantino', 'Steven Spielberg', 'Sidney Lumet', 'Thomas Kail', 'Christopher Nolan', 'David Fincher', 'Peter Jackson', 'Robert Zemeckis', 'Sergio Leone', 'Peter Jackson', 'Lana Wachowski', 'Martin Scorsese', 'Irvin Kershner', 'Milos Forman', 'Masaki Kobayashi', 'Bong Joon Ho', 'Christopher Nolan', 'Fernando Meirelles', 'Hayao Miyazaki', 'Steven Spielberg', 'Frank Darabont', 'Roberto Benigni', 'David Fincher', 'Jonathan Demme', 'George Lucas', 'Akira Kurosawa', 'Frank Capra', 'Todd Phillips', 'Damien Chazelle', 'Olivier Nakache', 'Christopher Nolan', 'Martin Scorsese', 'Roman Polanski', 'Ridley Scott', 'Tony Kaye', 'Bryan Singer', 'Luc Besson', 'Roger Allers', 'James Cameron', 'Giuseppe Tornatore', 'Isao Takahata', 'Robert Zemeckis', 'Hrishikesh Mukherjee', 'Sergio Leone', 'Alfred Hitchcock']
```

Fantastic, we will next make a function for each block of code (which is not shown here) and create a for loop to navigate through all the webpages to retrieve information from all 250 movies. 

```

movie_titles = []
year_released = []
genres = []
certification = []
imdb_rating = []
director = []
dom_gross = []
url = ["https://www.imdb.com/search/title/?groups=top_250&sort=user_rating", 
                'https://www.imdb.com/search/title/?groups=top_250&sort=user_rating,desc&start=51&ref_=adv_nxt',
                'https://www.imdb.com/search/title/?groups=top_250&sort=user_rating,desc&start=101&ref_=adv_nxt',
                'https://www.imdb.com/search/title/?groups=top_250&sort=user_rating,desc&start=151&ref_=adv_nxt',
                'https://www.imdb.com/search/title/?groups=top_250&sort=user_rating,desc&start=201&ref_=adv_nxt',]

for i in url:      

    response = requests.get(i)
    soup = BeautifulSoup(response.content, 'html.parser')
    movie_titles += retrieve_movietitle(soup)
    year_released += retrieve_year(soup)
    genres += retrieve_genre(soup)
    certification += retrieve_cert(soup)
    imdb_rating += retrieve_imdb(soup)
    director += retrieve_director(soup)
    dom_gross += retrieve_domestic(soup)
        
df = pd.DataFrame([movie_titles, year_released, genres, certification, imdb_rating, director, dom_gross]).transpose()
df.columns = ['Title', 'Year_Released', 'Genres', 'Movie_Rating', 'IMDB_Rating', 'Director', 'Domestic_Gross(millions)']
print (len(df))
display (df.head())
df.tail()
```

Boom! We just created our own dataframe with the neccessary information from IMDB Top 250 using BeautifulSoup web scraping. Now, we can create graphs, analysis our information and arrive at a conclusion to what makes the best movie studio.  


