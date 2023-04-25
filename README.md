# This is my Flask Application

## The file __init__ in flaskr
- __name__: is a built-in variable which evaluates to the name of the current module. </br>
- The app needs to know where it’s located to set up some paths, and __name__ is a convenient way to tell it that </br>
- "instance_relative_config=True" tells the app that configuration files are relative to the instance folder. </br>
- "SECRET_KEY" is used by Flask and extensions to keep data safe. It’s set to 'dev' to provide a convenient value during development, but it should be overridden with a random value when deploying. </br>
- DATABASE is the path where the SQLite database file will be saved. It’s under app.instance_path, which is the path that Flask has chosen for the instance folder. </br>
- "app.config.from_pyfile()" overrides the default configuration with values taken from the config.py file in the instance folder if it exists. For example, when deploying, this can be used to set a real SECRET_KEY. </br>
- os.makedirs() ensures that app.instance_path exists. Flask doesn’t create the instance folder automatically, but it needs to be created because your project will create the SQLite database file there. </br>
#### Run the app
<code> flask --app flaskr run --debug </code> </br>

## The file db.py in flaskr
#### Connect to the Database
- g is a special object that is unique for each request. It is used to store data that might be accessed by multiple functions during the request. The connection is stored and reused instead of creating a new connection if get_db is called a second time in the same request.</br>
- "current_app" is another special object that points to the Flask application handling the request. Since you used an application factory, there is no application object when writing the rest of your code. get_db will be called when the application has been created and is handling a request, so current_app can be used. </br>
- sqlite3.connect() establishes a connection to the file pointed at by the DATABASE configuration key. </br>
- sqlite3.Row tells the connection to return rows that behave like dicts. This allows accessing the columns by name. </br>

#### Create the Tables
- open_resource() opens a file relative to the flaskr package, which is useful since you won’t necessarily know where that location is when deploying the application later. get_db returns a database connection, which is used to execute the commands read from the file.</br>
- click.command() defines a command line command called init-db that calls the init_db function and shows a success message to the user. You can read Command Line Interface to learn more about writing commands.</br>

#### Register with the Application
- app.teardown_appcontext() tells Flask to call that function when cleaning up after returning the response.</br>
- app.cli.add_command() adds a new command that can be called with the flask command. </br>

#### Initialize the Database File
<code> flask --app flaskr init-db </code> </br>
