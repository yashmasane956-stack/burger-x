import React, { useState } from "react";
import { motion } from "framer-motion";
import { Button } from "@/components/ui/button";
import { Card, CardContent } from "@/components/ui/card";
import { ShoppingCart, Sun, Moon, Plus, Minus, Trash } from "lucide-react";

const initialMenu = [
  { id: 1, name: "X-Classic", price: 8.99, category: "Burgers", image: "https://images.unsplash.com/photo-1568901346375-23c9450c58cd" },
  { id: 2, name: "Double X", price: 11.99, category: "Burgers", image: "https://images.unsplash.com/photo-1550547660-d9450f859349" },
  { id: 3, name: "X-Fries", price: 3.99, category: "Sides", image: "https://images.unsplash.com/photo-1576107232684-1279f390859f" },
  { id: 4, name: "X-Shake", price: 4.99, category: "Drinks", image: "https://images.unsplash.com/photo-1572490122747-3968b75cc699" },
];

export default function BurgerXWebsite() {
  const [darkMode, setDarkMode] = useState(false);
  const [menu, setMenu] = useState(initialMenu);
  const [cart, setCart] = useState([]);
  const [adminMode, setAdminMode] = useState(false);

  const addToCart = (item) => {
    setCart((prev) => {
      const existing = prev.find((i) => i.id === item.id);
      if (existing) {
        return prev.map((i) =>
          i.id === item.id ? { ...i, qty: i.qty + 1 } : i
        );
      }
      return [...prev, { ...item, qty: 1 }];
    });
  };

  const updateQty = (id, delta) => {
    setCart((prev) =>
      prev
        .map((item) =>
          item.id === id ? { ...item, qty: item.qty + delta } : item
        )
        .filter((item) => item.qty > 0)
    );
  };

  const total = cart.reduce((sum, item) => sum + item.price * item.qty, 0);

  return (
    <div className={darkMode ? "bg-black text-white" : "bg-yellow-50 text-gray-900"}>
      {/* Header */}
      <header className="flex justify-between items-center p-6 max-w-7xl mx-auto">
        <h1 className="text-3xl font-extrabold">BURGER - X</h1>
        <div className="flex gap-4">
          <Button onClick={() => setAdminMode(!adminMode)} variant="outline">Admin</Button>
          <Button onClick={() => setDarkMode(!darkMode)} variant="outline">
            {darkMode ? <Sun size={18} /> : <Moon size={18} />}
          </Button>
          <Button variant="outline">
            <ShoppingCart size={18} /> ({cart.length})
          </Button>
        </div>
      </header>

      {/* Hero */}
      <section className="text-center py-16 px-6">
        <motion.h2
          initial={{ opacity: 0, y: -20 }}
          animate={{ opacity: 1, y: 0 }}
          className="text-4xl md:text-6xl font-bold"
        >
          Engineered for Flavor. Built for Speed.
        </motion.h2>
        <p className="mt-6 text-lg max-w-2xl mx-auto">
          Order faster. Eat better. Smash your hunger goals with Burger - X.
        </p>
      </section>

      {/* Menu */}
      <section className="max-w-7xl mx-auto px-6 py-12">
        <h3 className="text-3xl font-bold mb-8 text-center">Menu</h3>
        <div className="grid md:grid-cols-3 gap-8">
          {menu.map((item) => (
            <Card key={item.id} className="rounded-2xl shadow-xl overflow-hidden">
              <img src={item.image} alt={item.name} className="h-48 w-full object-cover" />
              <CardContent className="p-6">
                <h4 className="text-xl font-bold">{item.name}</h4>
                <p className="mt-2 font-semibold">${item.price.toFixed(2)}</p>
                <Button className="mt-4 w-full" onClick={() => addToCart(item)}>
                  Add to Cart
                </Button>
              </CardContent>
            </Card>
          ))}
        </div>
      </section>

      {/* Cart */}
      <section className="max-w-4xl mx-auto px-6 py-12">
        <h3 className="text-2xl font-bold mb-6">Your Cart</h3>
        {cart.length === 0 && <p>Your cart is empty.</p>}
        {cart.map((item) => (
          <div key={item.id} className="flex justify-between items-center mb-4">
            <div>
              {item.name} x {item.qty}
            </div>
            <div className="flex items-center gap-2">
              <Button size="sm" onClick={() => updateQty(item.id, -1)}>
                <Minus size={14} />
              </Button>
              <Button size="sm" onClick={() => updateQty(item.id, 1)}>
                <Plus size={14} />
              </Button>
              <Button size="sm" variant="destructive" onClick={() => updateQty(item.id, -item.qty)}>
                <Trash size={14} />
              </Button>
            </div>
          </div>
        ))}
        {cart.length > 0 && (
          <div className="mt-6">
            <h4 className="text-xl font-bold">Total: ${total.toFixed(2)}</h4>
            <Button className="mt-4 w-full text-lg py-4">Checkout</Button>
          </div>
        )}
      </section>

      {/* Admin Dashboard */}
      {adminMode && (
        <section className="max-w-4xl mx-auto px-6 py-12 border-t">
          <h3 className="text-2xl font-bold mb-6">Admin Dashboard</h3>
          <Button
            onClick={() =>
              setMenu([
                ...menu,
                {
                  id: Date.now(),
                  name: "New X Burger",
                  price: 9.99,
                  category: "Burgers",
                  image: "https://images.unsplash.com/photo-1553979459-d2229ba7433b",
                },
              ])
            }
          >
            Add New Item
          </Button>
        </section>
      )}

      {/* Footer */}
      <footer className="text-center py-8 border-t mt-12">
        © {new Date().getFullYear()} Burger - X. All rights reserved.
      </footer>
    </div>
  );
}
