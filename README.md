# Anota-es-2
const express = require('express');
require('dotenv').config();

const authRoutes = require('./routes/authRoutes');
const productRoutes = require('./routes/productRoutes');
const userRoutes = require('./routes/userRoutes');
const favoriteRoutes = require('./routes/favoriteRoutes');
const historyRoutes = require('./routes/historyRoutes');
const errorHandler = require('./middlewares/errorHandler');

const app = express();

// Middlewares
app.use(express.json());

// Routes
app.use('/auth', authRoutes);
app.use('/products', productRoutes);
app.use('/users', userRoutes);
app.use('/favorites', favoriteRoutes);
app.use('/history', historyRoutes);

// Health check
app.get('/health', (req, res) => {
  res.json({ 
    status: 'OK', 
    timestamp: new Date().toISOString(),
    uptime: process.uptime()
  });
});

// Rota principal
app.get('/', (req, res) => {
  res.json({ 
    message: '🚀 API FoodScan funcionando!',
    version: '1.0.0',
    endpoints: {
      auth: '/auth',
      products: '/products',
      users: '/users',
      favorites: '/favorites',
      history: '/history',
      health: '/health'
    },
    documentation: 'Consulte a documentação para mais detalhes'
  });
});

// Error handler (deve ser o último middleware)
app.use(errorHandler);

module.exports = app;
