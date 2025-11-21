
# 🏬 Warehouse Management System – FastAPI Microservices + Redis + ReactJS

The **Warehouse Management System (WMS)** is a full-stack application built using **FastAPI**, **Microservices Architecture**, **Redis (Key-Value + Streams)**, and **ReactJS**.
It supports real-time product management, order creation, inventory updates, and microservice communication via Redis Streams.

---

## 🚀 Project Overview

### **Tech Stack**

**Backend:**

* FastAPI
* Python 3.x
* Redis OM (Hash Models)
* Redis Streams for async communication
* Requests (service-to-service communication)
* Uvicorn (ASGI server)

**Frontend:**

* ReactJS
* React Router
* JavaScript + JSX
* CSS modules

**Database:**

* Redis Cloud (Key-Value store + Streams)

**Architecture:**

* **Warehouse Microservice (8000)** → Manages *products*
* **Store Microservice (8001)** → Manages *orders*
* **Redis Streams** → Syncs order events & updates inventory
* **Web UI (3000)** → React frontend displaying products & orders

---

## 🌐 Microservices Architecture

<img width="1036" height="568" alt="microservices architechture" src="https://github.com/user-attachments/assets/2e62fe20-8e7d-4b68-80aa-5924cde3d7a9" />
●	Warehouse microservice will basically manage products and these products will have a few parameters. With Fast API, we create products, retrieve products information and delete the products.
●	Store microservice will basically contact the warehouse and will create orders for products. [create orders, retrieve orders & information from warehouse. 
●	Redis database, which is popular, very efficient & easy to use database [key value store] Also, use streams which a way to communicate between our two microservices.
●	Response is sent to Web UI with React JS.

### **1️⃣ Warehouse Microservice**

Handles:

* Create product
* Fetch single product
* Fetch all products
* Delete product
* Update product quantity (via Redis Streams consumer)

### **2️⃣ Store Microservice**

Handles:

* Create order for a product
* Fetch all orders
* Fetch order by ID
* Publishes order events to Redis Streams (`order-completed`, `refund-order`)

### **3️⃣ Redis Streams**

Used for:

* Real-time inventory updates
* Order refund workflow
* Event-driven microservice communication

### **4️⃣ Frontend (ReactJS)**

Provides:

* Product listing
* Add product
* Delete product
* Place order
* Real-time price calculation

---

## ⚙️ Installation

### **1️⃣ Clone the Repository**

```bash
git clone https://github.com/yourusername/warehouse-management-system.git
cd warehouse-management-system
```

---

## 🛠 Backend Setup (FastAPI)

### **2️⃣ Create and Activate Virtual Environment**

```bash
py -m venv env
env\Scripts\activate.bat   # Windows
```

---

### **3️⃣ Install Dependencies**

```bash
pip install -r requirements.txt
```

If Redis OM errors occur:

```bash
pip install redis-om==0.0.26
```

---

### **4️⃣ Run Warehouse Microservice**

```bash
uvicorn main:app --reload --port 8000
```

### **5️⃣ Run Store Microservice**

```bash
uvicorn main:app --reload --port 8001
```

---

## 🧩 Backend API Endpoints

### **Warehouse (8000)**

| Method | Endpoint        | Description       |
| ------ | --------------- | ----------------- |
| POST   | `/product`      | Create product    |
| GET    | `/product/{pk}` | Get product by ID |
| GET    | `/products`     | Get all products  |
| DELETE | `/product/{pk}` | Delete product    |

---

### **Store (8001)**

| Method | Endpoint       | Description     |
| ------ | -------------- | --------------- |
| POST   | `/orders`      | Create order    |
| GET    | `/orders/{pk}` | Get order by ID |
| GET    | `/orders`      | Get all orders  |

---

## 🔄 Redis Streams Consumers

### **1️⃣ Order Completion Stream**

File: `fulfilment.py`

* Listens to `order-completed`
* Reduces product quantity in Redis
* Updates warehouse inventory in real-time

### **2️⃣ Order Refund Stream**

File: `update.py`

* Listens to `refund-order`
* Marks order as `refunded`

Run consumers:

```bash
python fulfilment.py
python update.py
```

---

## 🖥️ Frontend Setup (ReactJS)

### **1️⃣ Create React App**

```bash
cd fastapi_web
npx create-react-app .
npm install react-router-dom
npm start
```

### **2️⃣ Main Features**

* Product list table
* Create product form
* Delete product
* Create order form
* Real-time price calculation using FastAPI

---

## 📁 Project Structure

```text
warehouse/
├── main.py                  # Warehouse FastAPI
├── fulfilment.py            # Redis Stream consumer
store/
├── main.py                  # Store FastAPI
│
fastapi_web/                 # React frontend
├── src/
│   ├── routes/
│   │   ├── Products.js
│   │   ├── ProductCreate.js
│   │   ├── Order.js
│   ├── App.js
│   ├── App.css
```
<img width="1024" height="1024" alt="Warehouse Management System" src="https://github.com/user-attachments/assets/26f2ec40-d3d8-4cb7-9d1b-99fe88a341c3" />

---

## 🔐 Security & Best Practices

* Avoid storing Redis credentials directly in code (use `.env`).
* Use Docker for scalable microservices deployment.
* Add validation for user inputs in React forms.
* Protect backend endpoints using authentication in production.

---

## 🔧 Future Enhancements

* JWT Authentication for Admin Panel
* Docker Compose for multi-container deployment
* Add order history logs
* Multi-user role support (Admin, Manager, Staff)
* Implement WebSockets for live updates
* Replace Redis with PostgreSQL for long-term storage

