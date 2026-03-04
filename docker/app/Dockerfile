# docker/app/Dockerfile
FROM python:3.13-slim-trixie
EXPOSE 8080

# Set environment variables
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

# Afegim un usuari i un grup no privilegiats abans de copiar els fitxers. Empram -m per crear el home i el -r perque sigui un usuari de sistema.
RUN groupadd -g 1000 appgroup && \
    useradd -u 1000 -g appgroup -m -s /bin/bash appuser

# Set work directory
WORKDIR /app

# Install system dependencies
RUN apt-get update && apt-get install -y \
    curl \
    && rm -rf /var/lib/apt/lists/*

# Install Python dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
RUN pip install --no-cache-dir uvicorn

# Copy project
COPY sportsclub/ .

# Collect static files (needed when serving through a reverse proxy)
RUN python manage.py collectstatic --noinput

# Aseguram que appuser es el propietari dels arxius per si ha d'escriure logs o arxius temporals.
RUN chown -R appuser:appgroup /app

# Cambiam l'usuari al no privilegiat
USER appuser

# Run uvicorn
CMD ["uvicorn", "sportsclub.asgi:application", "--host", "0.0.0.0", "--port", "8080"]
