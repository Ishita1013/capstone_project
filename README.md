# capstone_project
# 📝 Dynamic Real-Time Pricing for Urban Parking Spaces

## 📌 Objective
To develop a **dynamic pricing model** for 14 urban parking spaces based on real-time data collected over 73 days. The model adapts parking prices in real-time based on occupancy, traffic, queue length, vehicle type, and competition.

---

## 📊 Dataset Overview

- **Duration:** 73 days  
- **Frequency:** Every 30 minutes (18 time points per day)  
- **Locations:** 14 parking lots  
- **Total Records:** 18,368  
- **Key Features:**
  - `Occupancy`, `Capacity`, `QueueLength`
  - `VehicleType` (car, bike, truck)
  - `TrafficConditionNearby` (low, medium, high)
  - `IsSpecialDay` (0/1)
  - GPS coordinates (latitude/longitude)

---

## ✅ Step-by-Step Models

### 🧮 Model 1: Baseline Linear Pricing

**Formula:**
Price_{t+1} = Price_t + α * (Occupancy / Capacity)

yaml
Copy
Edit

- **Base Price:** $10  
- **α (Alpha):** 5  
- Linear price increase with occupancy  
- Easy to implement and acts as a reference model

---

### 📈 Model 2: Demand-Based Pricing

**Demand Function:**
Demand = α·(Occupancy / Capacity) + β·QueueLength - γ·Traffic + δ·IsSpecialDay + ε·VehicleTypeWeight

ruby
Copy
Edit

- Parameters:  
  - α = 1.0, β = 0.5, γ = 0.8, δ = 2.0, ε = 1.5  
- Mappings:
  - `TrafficLevel`: {'low': 1, 'medium': 2, 'high': 3}  
  - `VehicleTypeWeight`: {'car': 1.0, 'bike': 0.5, 'truck': 1.5}

**Price Formula:**
Price_t = BasePrice * (1 + λ * NormalizedDemand)

yaml
Copy
Edit
- λ = 0.2  
- Prices clipped between $5 and $20 for stability

---

### 🧠 Model 3: Competitive Pricing

**Enhancement:** Adjust prices based on proximity to other lots and their pricing

**Logic:**
- If own lot is **full** and **more expensive** → reduce price by 5%
- If **competitor prices are higher** → increase price by 5%
- Use **geopy** to calculate distance between lots (within 1km)

---

## 📚 Assumptions

1. Queue length reflects demand overflow  
2. Traffic congestion reduces demand  
3. Special days increase parking demand  
4. Trucks are charged more due to higher impact  
5. Nearby = within 1 km  
6. No missing values assumed in the dataset

---

## 💵 How Price Changes with Demand & Competition

| Condition                           | Price Behavior        |
|------------------------------------|------------------------|
| High occupancy                     | Increase               |
| Long queues + special day          | Increase               |
| Nearby lots cheaper & full lot     | Decrease               |
| Nearby lots expensive              | Increase slightly      |
| Vehicle is a truck                 | Increase               |

---

## 📊 Visualization

- Real-time interactive line plots (via **Bokeh**)  
- Each plot includes hover info:
  - Time
  - Price (Model 1, 2, 3)
  - Occupancy
  - Queue Length
  - Normalized Demand
  - Vehicle Type
- Side-by-side plots for all three models

---

## 🛠️ Tools Used

- **Python**, **Pandas**, **NumPy**: Data wrangling  
- **Bokeh**: Visualization  
- **geopy**: Geo-distance calculations  
- **Pathway** (planned): Real-time stream simulation  

---

## 🧾 Summary

This project builds a **multi-layered real-time pricing engine** for urban parking. It evolves from simple occupancy-based adjustments (Model 1) to demand-aware pricing (Model 2), and finally, to a market-aware pricing strategy (Model 3) that factors in geographical competition and real-time load.

The result is a scalable and interpretable pricing model suitable for smart city infrastructure.

---
