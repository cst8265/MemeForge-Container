# ============================================================================
# CST8265 Lab 7 — INSECURE Dockerfile for MemeForge (GIVEN TO STUDENTS)
# ============================================================================
# A teammate wrote this to containerize MemeForge for deployment. It builds
# and runs, which is exactly why its problems go unnoticed. Review it against
# the Week 9 container-security notes and rewrite it. There are many distinct
# security problems in this file.
#
# Build from the repository root:  docker build -f docker/Dockerfile .
# DO NOT just skim it: trace each instruction and ask "what does an attacker
# gain from this?"
# ============================================================================

# Start from "whatever PHP+Apache is newest today"
FROM php:apache

# Install a grab-bag of tools "in case we need to debug in production"
RUN apt-get update && apt-get install -y \
      curl \
      wget \
      vim \
      git \
      sudo \
      sqlite3 \
      netcat-openbsd \
      iputils-ping

# Use the development php.ini "so we can see errors on the server"
RUN cp "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini"

# Settings for features coming next sprint, baked right into the image
ENV APP_ENV=production
ENV ADMIN_PASSWORD=Sup3rSecret!
ENV SESSION_SECRET=memeforge-session-key-9f3a
ENV MAIL_API_KEY=live_8c21f0b4d9e74a11

# Copy the entire project folder into Apache's web root.
# (The site is then at http://server/public/)
COPY . /var/www/html/

# Create and seed the database so the site isn't empty on launch
RUN php /var/www/html/database/build_db.php

# "Fixes the permission errors when users upload images"
RUN chmod -R 777 /var/www/html

# Install PHP dependencies
RUN composer install || true

EXPOSE 80

# (no USER instruction — Apache and PHP run as root)
