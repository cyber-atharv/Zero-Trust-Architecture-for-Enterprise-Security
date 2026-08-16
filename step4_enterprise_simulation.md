# Step 4: Simulate an Enterprise Security Environment

To accurately test a Zero Trust framework, we simulate an enterprise network utilizing containerization tools like Docker.

## Using Docker for Simulation

Docker allows us to deploy isolated environments that represent various enterprise resources (e.g., a web application, an admin dashboard, a database).

1. **Dockerized Applications:**
   - We create a custom `Dockerfile` that packages our application logic, web server (e.g., Nginx), and access control rules.
   
2. **Deployment Commands:**
   - Build the container image:
     ```bash
     docker build -t zta-security .
     ```
   - Run the container mapped to a specific port:
     ```bash
     docker run -d -p 9090:80 zta-security
     ```

3. **Validation:**
   - The application is then accessible at `http://localhost:9090`.
   - Access controls defined inside the container simulate network boundaries and authentication gateways.
