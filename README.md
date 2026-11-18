# Od-Telemiddle 📂

A lightweight middleware solution that decouples binary file storage from Odoo, offloading heavy media (images, PDFs, Excel sheets) from PostgreSQL directly into **Telegram Cloud Storage** via a private channel.

---

##  Tech Stack

* **Backend**: ![Java](https://img.shields.io/badge/Java-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white) ![Spring Boot](https://img.shields.io/badge/Spring_Boot-6DB33F?style=for-the-badge&logo=spring-boot&logoColor=white) ![Spring Data JPA](https://img.shields.io/badge/Spring_Data_JPA-6DB33F?style=for-the-badge&logo=spring&logoColor=white)
* **Database**: ![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
* **Cloud Storage**: ![Telegram](https://img.shields.io/badge/Telegram-26A5E4?style=for-the-badge&logo=telegram&logoColor=white)
* **Client Frontend**: ![Odoo](https://img.shields.io/badge/Odoo-714B67?style=for-the-badge&logo=odoo&logoColor=white)

---

##  Architecture Overview
* **Odoo (Client-Side):** Acts as a lightweight UI client. It intercepts file uploads, forwards them to the middleware, and stores **zero binary blobs and zero mapping tables** in its database.
* **Middleware Server (Spring Boot):** Exposes REST API endpoints (`/api/upload` and `/api/image`), handles Telegram Bot API integration, and maintains a lightweight **MySQL mapping database** linking `(res_model, res_id)` to Telegram `file_id`.
* **Telegram Platform (Storage):** Acts as the primary cloud storage bucket using a private Telegram channel.

---

##  Configuration (`application.properties`)

```properties
# MySQL Mapping Database
spring.datasource.url=jdbc:mysql://localhost:3306/odoo_telegram_middleware?useSSL=false&serverTimezone=UTC
spring.datasource.username=root
spring.datasource.password=your_mysql_password

spring.jpa.hibernate.ddl-auto=update

# File Upload Limits
spring.servlet.multipart.max-file-size=50MB
spring.servlet.multipart.max-request-size=50MB

# Telegram Bot Credentials
telegram.bot.token=YOUR_TELEGRAM_BOT_TOKEN
telegram.chat.id=YOUR_PRIVATE_CHANNEL_CHAT_ID
middleware.base.url=http://localhost:8080