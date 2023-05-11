FROM postgres:15.2-alpine

# Copy in the extensions script
COPY extensions.sh /docker-entrypoint-initdb.d/

# Set permissions
RUN chmod 755 /docker-entrypoint-initdb.d/extensions.sh
