# Deploying a uWSGI Application English

In this session, we will set up a Django 2 application using uWSGI binary protocol. We will utilize Python 3.6 and MariaDB as the database. The setup process is largely automated, but some manual steps are required.

### Prerequisites

1. **Python 3.6 Installation**:
    - Install Python 3.6 using yum:
        
        ```bash
        yum install python36 python36-devel
        
        ```
        
2. **PIP Installation**:
    - Download and install PIP using a bootstrapping script:
        
        ```bash
        wget <https://bootstrap.pypa.io/get-pip.py>
        python3.6 get-pip.py
        
        ```
        
    - Remove the bootstrapping script after installation:
        
        ```bash
        rm get-pip.py
        
        ```
        
3. **Install pipenv and requests**:
    - Install project management tools:
        
        ```bash
        pip3.6 install pipenv requests
        
        ```
        

### Setting Up the Django Application

1. **Clone the Repository**:
    - Clone the sample application repository:
        
        ```bash
        git clone <repository_url> /srv/www/content-uwsgi
        
        ```
        
2. **Database Configuration**:
    - Access MariaDB and create a new database and user:
        
        ```sql
        mysql -u root -p
        CREATE DATABASE django_notes;
        GRANT ALL PRIVILEGES ON django_notes.* TO 'notes'@'localhost' IDENTIFIED BY 'p@ssw0rd';
        FLUSH PRIVILEGES;
        
        ```
        
    - Remember the database credentials:
        - **Database Name**: django_notes
        - **User**: notes
        - **Password**: p@ssw0rd
3. **Environment Variables Setup**:
    - Set environment variables required for database migration:
        
        ```bash
        export NOTES_DB=django_notes
        export NOTES_DB_USER=notes
        export NOTES_DB_PASSWORD=p@ssw0rd
        
        ```
        
4. **Run Make Commands**:
    - Navigate to the application directory:
        
        ```bash
        cd /srv/www/content-uwsgi
        ```
        
    - Install dependencies:
        
        ```bash
        make install
        ```
        
    - Migrate the database:
        
        ```bash
        make migrate
        ```
        
    - Generate static assets:
        
        ```bash
        make static
        ```
        
    - Set up the service:
        
        ```bash
        make service
        ```
        

### Configuring the Service

1. **Service Setup**:
    - The `make service` command will prompt for the following information:
        - **Host**: [notes.example.com](http://notes.example.com/)
        - **Database Name**: django_notes
        - **Database User**: notes
        - **Password**: p@ssw0rd
2. **Enable and Start the Service**:
    - Enable and start the uWSGI service:
        
        ```bash
        systemctl start notes.uwsgi
        systemctl enable notes.uwsgi
        ```