# ── SecurePortal Docker Image ──
# Lightweight nginx container that serves the three-page portal.

FROM nginx:alpine

# Remove default nginx page
RUN rm -rf /usr/share/nginx/html/*

# Copy site files
COPY login.html            /usr/share/nginx/html/login.html
COPY admin_dashboard.html  /usr/share/nginx/html/admin_dashboard.html
COPY employee_dashboard.html /usr/share/nginx/html/employee_dashboard.html

# Copy custom nginx config
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Expose port 80
EXPOSE 80

# Healthcheck
HEALTHCHECK --interval=30s --timeout=5s --start-period=5s --retries=3 \
  CMD wget -qO- http://localhost/ || exit 1

CMD ["nginx", "-g", "daemon off;"]
