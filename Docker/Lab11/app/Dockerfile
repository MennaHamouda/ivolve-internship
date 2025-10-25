# Base image
FROM python:3.10

# Environment variables for production (build-time)
ENV APP_MODE=production
ENV APP_REGION=canada-west

# Set Work Directory
WORKDIR /app

# Copy Application
COPY . /app

#  Install Dependencies
RUN pip install flask

# Expose Port
EXPOSE 8080

# Run Flask App
CMD ["python", "app.py"]
