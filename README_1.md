# Crop Prediction & Farmer Support Platform

A Django-based web application built to help farmers make better decisions about what to grow, how to treat crop diseases, what fertilizer to use, and how to sell their produce — all in one place.

## Overview

This project combines machine learning models with a full web platform to support farmers end-to-end: from choosing a crop and diagnosing plant diseases, to recommending fertilizers and connecting with buyers through a built-in marketplace.

## Features

- **Crop Recommendation** — Suggests the best crop to grow based on soil nutrients (N, P, K), temperature, humidity, pH, and rainfall, using a trained Naive Bayes model.
- **Fertilizer Recommendation** — Recommends the right fertilizer from the same soil and climate inputs.
- **Plant Disease Detection** — Users upload a photo of a crop leaf; a VGG19-based deep learning model (Keras/TensorFlow) classifies the disease and returns a description and prevention advice.
- **Farmer & Vendor Accounts** — Separate registration/login flows for farmers and vendors, with profile editing and OTP-based password reset (via email).
- **E-Marketplace** — Farmers can list crops for sale (with images, quantity, price, and location); vendors can browse listings, add items to a cart, and send inquiries. Farmers and vendors can message each other about listings.
- **Dashboards & Reports** — Farmers can view their past crop/fertilizer prediction history and reports.
- **Agri News** — Pulls current agriculture-related news articles via the NewsAPI.
- **Static Info Pages** — Home, About, Services, Blog, Contact, Insurance, and Loan information pages.

## Tech Stack

- **Backend:** Django (Python)
- **Database:** SQLite
- **Machine Learning:** scikit-learn (Naive Bayes, via pickle files) for crop/fertilizer prediction; TensorFlow/Keras (VGG19) for leaf disease image classification
- **Frontend:** Django templates with Bootstrap, jQuery
- **Email:** Django's email backend (used for OTP password reset)

## Project Structure

```
crop_prediction/
├── app/                     # Main Django app (views, models, auth, urls)
├── crop_prediction/         # Django project settings, root URLs, WSGI/ASGI
├── dataset/                 # Trained models (.pkl, .h5) and training notebooks
├── templates/               # HTML templates for all pages
├── static/                  # CSS, JS, images
├── media/                   # User-uploaded images (crop listings, disease photos)
├── manage.py
└── db.sqlite3
```

## Setup Instructions

1. **Clone/extract the project** and navigate into the `crop_prediction` folder.

2. **Create a virtual environment and install dependencies:**
   ```bash
   python -m venv venv
   source venv/bin/activate   # On Windows: venv\Scripts\activate
   pip install django numpy tensorflow keras scikit-learn requests
   ```

3. **Apply database migrations:**
   ```bash
   python manage.py migrate
   ```

4. **Run the development server:**
   ```bash
   python manage.py runserver
   ```

5. Open `http://127.0.0.1:8000/` in your browser.

## Important Notes Before Deploying

- `DEBUG = True` and `SECRET_KEY` is hardcoded in `settings.py` — change both before any real deployment.
- The NewsAPI key used in `search_news` is hardcoded in `views.py` — move it to an environment variable and rotate the key, since it's currently exposed in the source code.
- Trained model files (`best_model.h5`, `model_sev.h5`) are large (~60–90 MB each) — consider Git LFS or external storage if you plan to version this with Git.
- Configure `EMAIL_*` settings properly for the OTP password-reset feature to work in production.

## Suggested Next Steps

- Add a `requirements.txt` (see packages listed above) so setup is reproducible.
- Move secrets (Django `SECRET_KEY`, NewsAPI key, email credentials) into environment variables.
- Add automated tests (an empty `tests.py` currently exists as a placeholder).
