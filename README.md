# Inventory Management System

![Inventory Banner](https://img.freepik.com/free-vector/warehouse-storage-illustration_1284-5654.jpg)  
*A robust solution to streamline your inventory operations.*

---

## Overview

The **Inventory Management System** is designed to efficiently manage stock, suppliers, and transactions for businesses of all sizes. Built with a modern tech stack, it offers both a powerful backend API and a user-friendly web interface.

---

## Tech Stack

- **Backend:** Java (Spring Boot)  
- **Frontend:** TypeScript, HTML, LESS, CSS  
- **Mock Data/Testing:** [ng-alain/mock](https://ng-alain.com/mock)

---

## Features

- 📦 **Stock Management:** Track goods in, goods out, and current levels.
- 🏷️ **Supplier & Product Catalog:** Organize products and suppliers for quick access.
- 📊 **Reporting:** Visualize stock movements and trends.
- 🔒 **User Authentication:** Secure access for staff and admins.
- 🔄 **Real-Time Updates:** Instantly reflect changes in inventory.

---

## Folder Structure

```text
- stocky-api/        # Java backend (Spring Boot)
- stocky-web/        # Frontend (Angular, TypeScript)
    └── _mock/       # Mock data for frontend testing
    └── src/app/core # Core Angular modules
```

---

## Getting Started

### 1. Clone the Repository
```bash
git clone https://github.com/khanrasidraja/-inventory-management-system.git
```

### 2. Backend Setup

```bash
cd stocky-api
# Install dependencies and run the server
./mvnw spring-boot:run
```

### 3. Frontend Setup

```bash
cd stocky-web
npm install
npm start
```

---

## Screenshots

> *Please upload your own screenshots for a personalized README.*

| Dashboard                    | Product List                | Stock Transaction           |
|------------------------------|-----------------------------|-----------------------------|
| ![Dashboard](assets/dashboard.png) | ![Products](assets/products.png) | ![Stock](assets/stock.png) |

---

## API Documentation

See [stocky-api/README.md](stocky-api/README.md) for backend API details.

---

## Mock Data

The frontend uses [ng-alain/mock](https://ng-alain.com/mock) for rapid prototyping. See `stocky-web/_mock/README.md` for details.

---

## Core Module

Angular's `CoreModule` centralizes essential services for the frontend. Details are in `stocky-web/src/app/core/README.md`.

---

## Contributing

Pull requests are welcome. For major changes, open an issue first to discuss what you would like to change.

---

## License

[MIT](LICENSE)

---

> **Made with ❤️ by [khanrasidraja](https://github.com/khanrasidraja)**
