# Uploading-Flask-App-Into-Cpanel

## Overview
This project aims to deploy a Flask web application on a cPanel-based server environment. The Flask application serves as a simple web interface, providing a landing page (`index.html`) with basic content. The deployment process includes setting up a virtual environment, installing necessary dependencies, uploading the project to cPanel, configuring a Python application, and handling common deployment errors.


### README.md File

#### Directory Hierarchy
```
/flask_app
    ├── app.py
    ├── static/
    │   └── css/
    │       └── styles.css
    ├── templates/
    │   └── index.html
    ├── venv/
    ├── requirements.txt
    └── README.md
```

#### Setting Up Your Flask Application

1. **Initialize Virtual Environment and Install Flask**
   ```bash
   # Create a virtual environment
   python -m venv venv

   # Activate the virtual environment (Windows)
   venv\Scripts\activate
   # Activate the virtual environment (Mac/Linux)
   source venv/bin/activate

   # Install Flask
   pip install Flask
   ```

2. **Create Your Flask Application (`app.py`)**
   ```python
   from flask import Flask, render_template

   app = Flask(__name__)

   @app.route('/')
   def index():
       return render_template('index.html')

   if __name__ == '__main__':
       app.run(debug=True)
   ```

3. **Create `index.html` in `templates` Folder**
   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>Flask App</title>
       <link rel="stylesheet" href="{{ url_for('static', filename='css/styles.css') }}">
   </head>
   <body>
       <h1>Welcome to Flask App</h1>
   </body>
   </html>
   ```

4. **Freeze Installed Packages to `requirements.txt`**
   ```bash
   pip freeze > requirements.txt
   ```

#### Deployment to cPanel

5. **Upload Project to cPanel**

   - Zip your `flask_app` directory.
   - Upload the zip file to cPanel File Manager and extract it.

6. **Create Python Application in cPanel**

   - Navigate to cPanel and find Python Applications.
   - Create a new application, specifying the Python version (e.g., 3.7.17 recommended) and application root (`flask_app`).
   - ![image](https://github.com/MuhammadRaheelNaseem/Uploading-Flask-App-Into-Cpanel/assets/63813881/2a1ff095-e8f2-437b-a68a-2453c519ea5e)


7. **Activate Virtual Environment and Install Requirements**

   - Access cPanel Terminal or SSH into your server.
   - Navigate to your application root.
   - Activate the virtual environment:
     ```bash
     source venv/bin/activate
     ```
   - Install requirements:
     ```bash
     pip install -r requirements.txt
     ```

8. **Update `passenger_wsgi.py`**

   - Edit or create `passenger_wsgi.py` in the application root to point to your Flask application:
     ```python
     import sys, os
     cwd = os.getcwd()
     sys.path.append(cwd)
     from app import app as application
     ```

9. **Restart Python Application**

   - In cPanel, restart your Python application to apply changes.

#### Handling Errors and Additional Configurations

10. **Handling Common Errors**

   - **Error: No matching distribution found**
     - If encountering specific version mismatches (e.g., Flask-WTF, WTForms), update versions in `requirements.txt`.
   
   - **Error: Could not build wheels for greenlet**
     - Update pip and setuptools, then install the wheel and greenlet packages.
   
   - **Error: ImportError: cannot import name 'DeclarativeBase'**
     - Adjust import statements for SQLAlchemy to `from sqlalchemy.ext.declarative import declarative_base`.

11. **Database Setup (Optional)**

   - Create and configure a PostgreSQL database using cPanel's PostgreSQL Database Wizard.

12. **Environment Variables Setup**

   - Add environment variables like `FLASK_KEY` in cPanel's Setup Python App section to enhance security.

13. **Checking Error Logs**

   - Monitor error logs (`stderr.log`) in cPanel to debug application issues effectively.

### Conclusion

This README.md file provides a comprehensive guide to setting up and deploying your Flask application on cPanel, addressing common errors and configurations. Customize the instructions based on your specific project requirements and server environment.

This structure should help you create a detailed and clear README.md file for your Flask application deployment. Adjust the instructions as necessary to fit any additional requirements or configurations unique to your project.
