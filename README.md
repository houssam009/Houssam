import React, { useState } from "react";

const initialServices = [
  {
    name: "Lynx",
    icon: "🦊",
    plans: [
      { duration: "شهر", price: "500 DA" },
      { duration: "3 أشهر", price: "1400 DA" },
      { duration: "6 أشهر", price: "2700 DA" },
      { duration: "عام", price: "5200 DA" },
    ],
  },
  {
    name: "Shahid",
    icon: "📺",
    plans: [
      { duration: "شهر", price: "800 DA" },
      { duration: "3 أشهر", price: "2200 DA" },
      { duration: "6 أشهر", price: "4200 DA" },
      { duration: "عام", price: "8000 DA" },
    ],
  },
];

export default function App() {
  const [services, setServices] = useState(initialServices);
  const [isAdmin, setIsAdmin] = useState(false);
  const [loginData, setLoginData] = useState({ email: "", password: "" });
  const [orders, setOrders] = useState([]);

  const handlePriceChange = (serviceIndex, planIndex, newPrice) => {
    const updatedServices = [...services];
    updatedServices[serviceIndex].plans[planIndex].price = newPrice;
    setServices(updatedServices);
  };

  const handleLogin = () => {
    if (loginData.email === "admin@example.com" && loginData.password === "123456") {
      setIsAdmin(true);
    } else {
      alert("بيانات الدخول غير صحيحة");
    }
  };

  const handlePurchase = (serviceName, plan) => {
    const newOrder = {
      service: serviceName,
      duration: plan.duration,
      price: plan.price,
      time: new Date().toLocaleString(),
    };
    setOrders([newOrder, ...orders]);
    alert("تم تسجيل الطلب بنجاح!");
  };

  return (
    <div style={{ maxWidth: 700, margin: "auto", padding: 20, fontFamily: "Arial" }}>
      {!isAdmin && (
        <div style={{ marginBottom: 20, padding: 15, background: "#f0f0f0", borderRadius: 8 }}>
          <h2>تسجيل دخول المسؤول</h2>
          <input
            type="email"
            placeholder="البريد الإلكتروني"
            value={loginData.email}
            onChange={(e) => setLoginData({ ...loginData, email: e.target.value })}
            style={{ width: "100%", padding: 8, marginBottom: 10 }}
          />
          <input
            type="password"
            placeholder="كلمة المرور"
            value={loginData.password}
            onChange={(e) => setLoginData({ ...loginData, password: e.target.value })}
            style={{ width: "100%", padding: 8, marginBottom: 15 }}
          />
          <button onClick={handleLogin} style={{ width: "100%", padding: 10 }}>
            دخول
          </button>
        </div>
      )}

      <h1 style={{ textAlign: "center" }}>عروض الاشتراك المميزة</h1>

      {services.map((service, serviceIndex) => (
        <div key={service.name} style={{ border: "1px solid #ccc", borderRadius: 8, padding: 15, marginBottom: 20 }}>
          <h2>{service.icon} {service.name}</h2>
          {service.plans.map((plan, planIndex) => (
            <div key={plan.duration} style={{ marginBottom: 10 }}>
              <strong>{plan.duration} - </strong>
              {isAdmin ? (
                <input
                  value={plan.price}
                  onChange={(e) => handlePriceChange(serviceIndex, planIndex, e.target.value)}
                  style={{ width: 100, marginRight: 10 }}
                />
              ) : (
                <span>{plan.price}</span>
              )}
              <button onClick={() => handlePurchase(service.name, plan)}>اشترِ الآن</button>
            </div>
          ))}
        </div>
      ))}

      {isAdmin && (
        <div>
          <h2>قائمة الطلبات</h2>
          {orders.length === 0 && <p>لا توجد طلبات بعد.</p>}
          <ul>
            {orders.map((order, index) => (
              <li key={index}>
                {order.service} - {order.duration} - {order.price} <br />
                <small>{order.time}</small>
              </li>
            ))}
          </ul>
        </div>
      )}
    </div>
  );
}
