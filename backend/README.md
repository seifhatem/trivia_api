# Full Stack Trivia API Backend

## Getting Started

### Installing Dependencies

#### Python 3.7

Follow instructions to install the latest version of python for your platform in the [python docs](https://docs.python.org/3/using/unix.html#getting-and-installing-the-latest-version-of-python)

#### Virtual Enviornment

We recommend working within a virtual environment whenever using Python for projects. This keeps your dependencies for each project separate and organaized. Instructions for setting up a virual enviornment for your platform can be found in the [python docs](https://packaging.python.org/guides/installing-using-pip-and-virtual-environments/)

#### PIP Dependencies

Once you have your virtual environment setup and running, install dependencies by naviging to the `/backend` directory and running:

```bash
pip install -r requirements.txt
```

This will install all of the required packages we selected within the `requirements.txt` file.

##### Key Dependencies

- [Flask](http://flask.pocoo.org/)  is a lightweight backend microservices framework. Flask is required to handle requests and responses.

- [SQLAlchemy](https://www.sqlalchemy.org/) is the Python SQL toolkit and ORM we'll use handle the lightweight sqlite database. You'll primarily work in app.py and can reference models.py.

- [Flask-CORS](https://flask-cors.readthedocs.io/en/latest/#) is the extension we'll use to handle cross origin requests from our frontend server.

## Database Setup
With Postgres running, restore a database using the trivia.psql file provided. From the backend folder in terminal run:
```bash
psql trivia < trivia.psql
```

## Running the server

From within the `backend` directory first ensure you are working using your created virtual environment.

To run the server, execute:

```bash
export FLASK_APP=flaskr
export FLASK_ENV=development
flask run
```

Setting the `FLASK_ENV` variable to `development` will detect file changes and restart the server automatically.

Setting the `FLASK_APP` variable to `flaskr` directs flask to use the `flaskr` directory and the `__init__.py` file to find the application.

## Tasks

One note before you delve into your tasks: for each endpoint you are expected to define the endpoint and response data. The frontend will be a plentiful resource because it is set up to expect certain endpoints and response data formats already. You should feel free to specify endpoints in your own way; if you do so, make sure to update the frontend or you will get some unexpected behavior.

1. Use Flask-CORS to enable cross-domain requests and set response headers.
2. Create an endpoint to handle GET requests for questions, including pagination (every 10 questions). This endpoint should return a list of questions, number of total questions, current category, categories.
3. Create an endpoint to handle GET requests for all available categories.
4. Create an endpoint to DELETE question using a question ID.
5. Create an endpoint to POST a new question, which will require the question and answer text, category, and difficulty score.
6. Create a POST endpoint to get questions based on category.
7. Create a POST endpoint to get questions based on a search term. It should return any questions for whom the search term is a substring of the question.
8. Create a POST endpoint to get questions to play the quiz. This endpoint should take category and previous question parameters and return a random questions within the given category, if provided, and that is not one of the previous questions.
9. Create error handlers for all expected errors including 400, 404, 422 and 500.

## API Endpoints

#### GET  '/api/categories'
- Fetches a dictionary of categories in which the keys are the ids and the value is the corresponding string of the category
- Request Arguments: None
- Returns: An object with a single key, categories, that contains a object of id: category_string key:value pairs.
```
{'1' : "Science",
'2' : "Art",
'3' : "Geography",
'4' : "History",
'5' : "Entertainment",
'6' : "Sports"}
```
#### GET  '/api/questions'
- Fetches an array with 3 dictionaries: categories (all available categories), questions (limited number of questions based on the QUESTIONS_PER_PAGE variable) & total_questions (count)
- Request Arguments: **OPTIONAL** ?page=x (where x is the page number)
	- /api/questions?page=3
- Returns: an array including
	- categories: key/value pairs
	- questions: array of question objects
	-  total_questions: questions count
```
{
  "categories": {
    "1": "Science",
    "2": "Art",
    "3": "Geography",
    "4": "History",
    "5": "Entertainment",
    "6": "Sports"
  },
  "questions": [
    {
      "answer": "Answer 1",
      "category": 4,
      "difficulty": 2,
      "id": 5,
      "question": "Question 1?"
    },
    {
      "answer": "Answer 2",
      "category": 4,
      "difficulty": 1,
      "id": 9,
      "question": "Question 2?"
    },
    .....
    .....
    .....
  ],
  "total_questions": 30
}
```

#### GET  'api/categories/*categoryid*/questions'
- Fetches an array with with the questions in the specified category and the number of questions
- Request Arguments: categoryid
- Returns: an array including
	- questions: array of question objects
	-  total_questions: questions count
	- current_category: the searched category
```
{
  "current_category": "1",
  "questions": [
    {
      "answer": "The Liver",
      "category": 1,
      "difficulty": 4,
      "id": 20,
      "question": "What is the heaviest organ in the human body?"
    },
    {
      "answer": "Alexander Fleming",
      "category": 1,
      "difficulty": 3,
      "id": 21,
      "question": "Who discovered penicillin?"
    },
   ...
   ...
   ...
  ],
  "total_questions": 12
}
```

#### POST  '/api/questions'
- Adds a new question to the database
- Request Arguments:
	- question: String
	- answer: String
	- category: the category id returned from api/categories (Int)
	- difficulty: 1-5 (Int)
```
{"question":"Question","answer":"Answer?","difficulty":"3","category":1}
```
- Returns: the new question object or error 500 in case of failure
```
{
  "result": {
    "answer": "Answer",
    "category": 1,
    "difficulty": 3,
    "id": 40,
    "question": "Question?"
  }
}
```
#### POST  '/api/questions/search'
- Search the questions using a substring of it
- Request Arguments:
	- searchTerm: part of the question (String)
```
{"searchTerm":"bla"}
```
- Returns: an array with the matching questions or empty in case of no matches with the search count
```
{
{
  "questions": [
    {
      "answer": "Edward Scissorhands",
      "category": 5,
      "difficulty": 3,
      "id": 6,
      "question": "What was the title of the 1990 fantasy directed by Tim Burton about a young man with multi-bladed appendages?"
    }
  ],
  "total_questions": 1
}
```
```
{
  "questions": [],
  "total_questions": 0
}
```

#### DELETE  '/api/questions/*questionid*'
- Deletes a question
- Request Arguments: the question id passed in the URI
- Returns: Deleting Confirmation or error 500
```
{
  "result": "Deleted Sucessfully"
}
```

#### POST  '/api/quizzes'
- Run a quiz based on the specified category
- Request Arguments:
	- previous_questions: questions previously shown to the play (Array of Int)
	- quiz_category: Category Object
```
{"previous_questions":[2],"quiz_category":{"type":"Entertainment","id":"5"}}
```
- Returns: a new question object that is not in the previous_questions array
```
{
  "question": {
    "answer": "Tom Cruise",
    "category": 5,
    "difficulty": 4,
    "id": 4,
    "question": "What actor did author Anne Rice first denounce, then praise in the role of her beloved Lestat?"
  }
}
```


## Testing
To run the tests, run
```
dropdb trivia_test
createdb trivia_test
psql trivia_test < trivia.psql
python test_flaskr.py
```
