# Module 14: BREAD Operations with FastAPI
![Coverage Badge](https://github.com/lcphutchinson/is601_14/actions/workflows/ci-cd.yml/badge.svg)

A module of is601 Web Systems Development, by Keith Williams

This module finalizes the project by implementing the final Calculations routes with a rich Tailwind frontend.

See the Dockerhub Repo [[Here]](https://hub.docker.com/repository/docker/lcphutchinson/is601_14)

## Reflection Component

Much of the Calculations functionality was complete as of Module 13, so there wasn't much to be changed beyond some minor compatibility tweaks for the new tailwind pages. While I have made a point throughout much of the course to closely rewrite all of the provided code 'in my own words', I have not done so with the template pages, as I am much less familiar with the structure of html and don't have the same means of debugging that I do with Python.

As of this writing, there are still some bad behaviors endemic to the architecture we've chosen to use in this project, particularly in parsing bad inputs that aren't checked for in the front-end logic (Divsion by Zero, for example)--none are dangers to the back-end state, just errors being thrown in an unhelpful way that prevents the front-end from displaying proper error messages. I may attempt to remedy this in the final if I have time, but I have squandered much of my week in burnout and have much to do before Monday.

My challenge this week arose while filling in some of my previously neglected end-to-end test cases, which for reasons that escape me run locally but hang when executed in the github workflow. Lacking the time for a proper investigation, I settled for probing from my new tests for the culprit (reflected in my commit history) and in a few tries located a pytest fixture causing the hanging. The fixture and its two dependent tests are commented out in tests/e2e/test_fastapi_calculator.py--if you have the time, uncomment them and try them locally: I'd be interested to know if they only work on my machine.

## Screenshots

### Github Workflow Execution
<img width="935" height="252" alt="Screenshot 2025-08-10 041503" src="https://github.com/user-attachments/assets/d5cda046-6fcd-43bc-b406-d43e16a522c2" />

### Docker Repository Upload
<img width="830" height="705" alt="Screenshot 2025-08-10 041702" src="https://github.com/user-attachments/assets/481471f0-2498-46f6-993c-84149f0e298b" />

### Endpoint Functionalities

- ADD/BROWSE, from dashboard

note the confirmation toasts (top) and retrieved calculations (bottom)
<img width="964" height="531" alt="Screenshot 2025-08-10 031949" src="https://github.com/user-attachments/assets/9241a71d-ad77-46c4-8871-49c416e695ab" />
<img width="963" height="601" alt="Screenshot 2025-08-10 032028" src="https://github.com/user-attachments/assets/33b032d9-4a7e-4e23-8c4d-8da8cf938e04" />
<img width="964" height="674" alt="Screenshot 2025-08-10 032047" src="https://github.com/user-attachments/assets/8cc9b075-07de-4185-ab7b-39b27d91a165" />
<img width="961" height="748" alt="Screenshot 2025-08-10 032125" src="https://github.com/user-attachments/assets/0742e1fd-8005-460c-a43f-903d3bc0949d" />

- READ, from single-view menu
<img width="962" height="578" alt="Screenshot 2025-08-10 032150" src="https://github.com/user-attachments/assets/16e93144-ad99-4c8c-b83e-86d71ef682f3" />

- EDIT, from single-view menu (post edit)

note the created datetime from READ and the new last updated
<img width="968" height="578" alt="Screenshot 2025-08-10 032219" src="https://github.com/user-attachments/assets/9d69a961-68b0-4443-9d6f-7f065920e176" />

- DELETE (in two parts)
<img width="982" height="830" alt="Screenshot 2025-08-10 032253" src="https://github.com/user-attachments/assets/ec8e7c3c-cd92-4ea5-8cc6-0090a9f188d4" />
<img width="966" height="679" alt="Screenshot 2025-08-10 032306" src="https://github.com/user-attachments/assets/b42f83ad-c52d-47d8-a472-028332352162" />

### Running the Test Suite

After cloning the repo to your local machine, create and enter virtual environment with the venv module

```bash
python3 -m venv venv
source venv/bin/activate
```

Then, from the root folder, install the project's requirements

```bash
pip install -r requirements.txt
playwright install
```

You may be prompted by playwright to install some dependencies it needs, but its instructions are clear.
Next, deploy the docker image in daemon mode to reserve your terminal for testing.

```bash
docker compose up --build -d
```

After configuration is finished, run the test suite

```bash
pytest
```

### Running the Calculator

After booting up the image, you can access the web page directly with `http://localhost:8000` and the API specification with `http://localhost:8000/docs`
The primary page contains links for registration and login, which will redirect you to the calculations dashboard. 

To create a calculation, select a calculation type from the dropdown and enter a list of comma-separated inputs into the input field. Note that all calculation types require at least two inputs. When all fields are to your liking, click 'Calculate' to submit your calculation, which will then appear in your Calculation history below the interface. Calculation records can be modified by selecting the 'Edit' button associated with the record in your history, or deleted with the delete button in the same cell. Editing a calculation will require submitting new input values, which can be provided in the edit window.
