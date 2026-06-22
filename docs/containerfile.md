# Use a lightweight Python 3.11 base image
FROM python:3.12      

# Set the working directory inside the container to /app
WORKDIR /app                 

# Copy requirements.txt into the container
COPY requirements.txt .      

# Install dependencies without caching
RUN pip install --no-cache-dir -r requirements.txt   

# Copy all project files into the container
COPY . .                     

# Expose port 8000 for external access
EXPOSE 8000                  

# Run the app with Uvicorn, listening on all interfaces at port 8000
CMD ["uvicorn","main:app","--host","0.0.0.0","--port","8000"]  
