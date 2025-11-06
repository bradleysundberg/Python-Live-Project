# Python-Live-Project
My Python live project for The Tech Academy's Software Developer Bootcamp

Overview:
Developed a full-stack Django application with basic Create, Read, and Detail functionality. The app enables users to add, view, and explore items stored in a database through a clean and responsive web interface.

Key Features:

Created a custom Django app with modular templates (base.html, home.html, details.html) for a consistent layout.

Built a data model in models.py and connected it to a form (ModelForm) so users can add new items directly through the site.

Implemented a list view that retrieves all database records and displays them dynamically in a structured table.

Added detailed item pages linked from the main list, using URL parameters and context data to render the correct entry.

Applied basic CSS styling for cohesive navigation, spacing, and color themes.

Technical Stack:
Django · Python · HTML · CSS · SQLite

Outcome:
Successfully deployed a functional prototype demonstrating core CRUD operations, clean template structure, and integration between Django models, views, and URLs.

# Lemur Finder Project Reflection

For this project, I created a Django web application called Lemur Finder, which allows users to explore, add, edit, and delete lemur information. The idea behind this project was to build a fully functional CRUD (Create, Read, Update, Delete) web app while practicing Django’s models, views, forms, templates, and routing.

# Project Overview

The Lemur Finder app starts with a home page that welcomes users and provides a link to view all lemurs. Users can navigate through the site using a consistent navbar across all pages, which includes links to Home, About, Gallery, and Contact sections (though some are placeholders for future content). The interface is styled using Bootstrap and a custom CSS file to give a friendly and visually appealing design.

# Key Features

Lemur Listing:
The lemur_list page displays all lemurs stored in the database in a table format, including details like name, species, location, and age. Each lemur has a “View Details” link to see more information.

Lemur Details:
The lemur_detail page shows a single lemur’s information, including species, description, and date added. Users can also edit or delete the lemur from this page.

Adding New Lemurs:
Using the create_lemur form, users can add new lemurs by providing their name, species, age, description, and an optional image URL. The form uses Django’s ModelForm for clean and efficient data handling.

Editing Lemurs:
Users can update lemur details through the edit_lemur page, which pre-fills the form with existing data. After submitting, changes are saved to the database.

Deleting Lemurs:
The delete_lemur page provides a confirmation step before removing a lemur from the database to prevent accidental deletions.

# Technical Highlights

Models: A single Lemur model defines the lemur data, including a choice field for species.

```python # models.py
from django.db import models

class Lemur(models.Model):
    SPECIES_CHOICES = [
        ('Ruffed', 'Ruffed Lemur'),
        ('Ring-tailed', 'Ring-tailed Lemur'),
        ('Sifaka', 'Sifaka'),
        ('Mouse', 'Mouse Lemur'),
    ]

    name = models.CharField(max_length=100)
    species = models.CharField(max_length=20, choices=SPECIES_CHOICES)
    age = models.PositiveIntegerField()
    description = models.TextField(blank=True)
    image_url = models.URLField(blank=True, null=True)

    def __str__(self):
        return f"{self.name} ({self.species})"
```


Forms: LemurForm is a ModelForm that handles both creating and editing lemurs, with custom widgets for a better user experience.

```python # forms.py
from django import forms
from .models import Lemur

class LemurForm(forms.ModelForm):
    class Meta:
        model = Lemur
        fields = ['name', 'species', 'age', 'description', 'image_url']
        widgets = {
            'description': forms.Textarea(attrs={'rows': 3, 'placeholder': 'Enter details about this lemur...'}),
            'name': forms.TextInput(attrs={'placeholder': 'Lemur name'}),
            'image_url': forms.URLInput(attrs={'placeholder': 'https://...'}),
        }
```

Views: Function-based views handle rendering templates and processing forms for each CRUD operation.

```python # views.py (example: create and detail)
from django.shortcuts import render, redirect, get_object_or_404
from .forms import LemurForm
from .models import Lemur

def create_lemur(request):
    if request.method == 'POST':
        form = LemurForm(request.POST)
        if form.is_valid():
            form.save()
            return redirect('lemur_home')
    else:
        form = LemurForm()
    return render(request, 'LemurFinder/create_lemur.html', {'form': form})

def lemur_detail(request, pk):
    lemur = get_object_or_404(Lemur, pk=pk)
    return render(request, 'LemurFinder/lemur_detail.html', {'lemur': lemur})
```

Templates: Reusable templates with a base layout (LemurFinder_base.html) and individual templates for home, list, detail, create, edit, and delete pages.

```markdown
<!-- create_lemur.html -->
{% extends 'LemurFinder/LemurFinder_base.html' %}

{% block content %}
<h2>Add a New Lemur</h2>
<form method="post">
    {% csrf_token %}
    {{ form.as_p }}
    <button type="submit">Save Lemur</button>
</form>
{% endblock %}
```

Routing: Clean URL patterns are set up to map each view to a friendly URL.

```python # urls.py
from django.urls import path
from . import views

urlpatterns = [
    path('', views.home, name='lemur_home'),
    path('create/', views.create_lemur, name='create_lemur'),
    path('lemurs/', views.lemur_list, name='lemur_list'),
    path('<int:pk>/', views.lemur_detail, name='lemur_detail'),
]
```

Styling: Bootstrap is combined with a custom CSS file to make the site responsive and visually appealing.

```markdown
```css
/* style.css */
body {
    background-color: #f0f8ff;
    font-family: Arial, sans-serif;
    text-align: center;
    margin: 0;
}

button {
    padding: 8px 14px;
    border-radius: 6px;
    border: none;
    background-color: #4CAF50;
    color: white;
    cursor: pointer;
}
```

# Challenges and Learning

One of the challenges I faced was managing the CRUD workflow correctly while keeping templates and forms clean and reusable. It was also interesting to integrate Bootstrap with Django templates and maintain consistent styling across multiple pages. I learned a lot about Django’s ModelForm, URL routing, and how to use generic views logic manually to handle forms and database operations.

# Conclusion

Overall, this project helped me solidify my understanding of Django’s core concepts, including models, views, templates, and forms. It was rewarding to see a fully functional web app come together that allows users to manage lemur information easily. I also gained practical experience in styling a Django app for a user-friendly interface and handling CRUD operations effectively.
