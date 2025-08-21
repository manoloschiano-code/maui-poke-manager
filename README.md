import React, { useState, useEffect } from 'react';
import { Calendar, Package, Truck, AlertCircle, FileText, Settings, Home, Plus, X, DollarSign, BarChart3, Users, Clock, Download, Upload, LogOut, Lock, User } from 'lucide-react';

const MauiPokeFoodCost = () => {
  // Stati per autenticazione
  const [isAuthenticated, setIsAuthenticated] = useState(false);
  const [username, setUsername] = useState('');
  const [password, setPassword] = useState('');
  const [currentUser, setCurrentUser] = useState('');
  
  // Stati principali
  const [activeTab, setActiveTab] = useState('dashboard');
  const [stores] = useState(['Lugano', 'Locarno', 'Grancia', 'Mendrisio', 'Lido', 'Lugano centro']);
  const [selectedStore, setSelectedStore] = useState('Lugano');
  const [currentDate] = useState(new Date().toLocaleDateString('it-IT'));
  
  // Database fornitori COMPLETO
  const [suppliers] = useState({
    'BIANCHI': {
      schedule: ['Martedì', 'Venerdì'],
      products: [
        { name: 'POLLO', unit: 'kg', price: 12.50 },
        { name: 'GAMBERI', unit: 'kg', price: 18.00 },
        { name: 'SALMONE', unit: 'kg', price: 22.00 },
        { name: 'TONNO', unit: 'kg', price: 25.00 },
        { name: 'SALMONE TRANCI', unit: 'pz', price: 3.30 },
        { name: 'POLPETTE VEGGI', unit: 'kg', price: 15.00 }
      ]
    },
    'TRANSGOURMET': {
      schedule: ['Mercoledì', 'Domenica'],
      products: [
        { name: 'Cous cous', unit: 'kg', price: 3.50 },
        { name: 'Uovo sodo', unit: 'pz', price: 0.35 },
        { name: 'Philadelphia', unit: 'kg', price: 8.50 },
        { name: 'Humus barbabietola', unit: 'kg', price: 7.20 },
        { name: 'Feta 0.2 kg', unit: 'pz', price: 2.80 },
        { name: 'Feta 0.9 kg', unit: 'pz', price: 9.50 },
        { name: 'Mais', unit: 'kg', price: 2.20 },
        { name: 'Germogli di soia', unit: 'kg', price: 3.80 },
        { name: 'Patate dolci', unit: 'kg', price: 2.90 },
        { name: 'Mandorle', unit: 'kg', price: 12.00 },
        { name: 'Avocado', unit: 'pz', price: 2.50 },
        { name: 'Mango', unit: 'pz', price: 3.20 },
        { name: 'Cetrioli', unit: 'kg', price: 2.80 },
        { name: 'Cipolla rossa', unit: 'kg', price: 1.90 },
        { name: 'Carote', unit: 'kg', price: 1.50 },
        { name: 'Cavolo rosso', unit: 'kg', price: 2.20 },
        { name: 'Insalata (misticanza)', unit: 'kg', price: 4.50 },
        { name: 'Pomodorini', unit: 'kg', price: 3.80 },
        { name: 'Vitamin Defence', unit: 'pz', price: 2.80 },
        { name: 'Vitamin Relax', unit: 'pz', price: 2.80 },
        { name: 'Vitamin Zero ignite', unit: 'pz', price: 2.80 },
        { name: 'Vitamin Fiber', unit: 'pz', price: 2.80 },
        { name: 'Coca Cola', unit: 'pz', price: 1.20 },
        { name: 'Coca Cola zero', unit: 'pz', price: 1.20 },
        { name: 'Sprite', unit: 'pz', price: 1.20 },
        { name: 'Fanta', unit: 'pz', price: 1.20 },
        { name: 'Acqua Valser frizzante', unit: 'pz', price: 0.80 },
        { name: 'Acqua Maui', unit: 'pz', price: 1.80 },
        { name: 'Nestea Limone', unit: 'pz', price: 1.50 },
        { name: 'Nestea Pesca', unit: 'pz', price: 1.50 },
        { name: 'Heineken', unit: 'pz', price: 2.50 },
        { name: 'Yuzu e Citro', unit: 'pz', price: 3.20 },
        { name: 'Acai berry', unit: 'pz', price: 3.20 },
        { name: 'Citro', unit: 'pz', price: 3.20 },
        { name: 'Goji', unit: 'pz', price: 3.20 },
        { name: 'Granade', unit: 'pz', price: 3.20 },
        { name: 'Acqua di cocco', unit: 'pz', price: 2.90 },
        { name: 'Yogurt senza lattosio', unit: 'kg', price: 4.50 },
        { name: 'Zucchero', unit: 'kg', price: 2.50 },
        { name: 'Sale', unit: 'kg', price: 1.20 },
        { name: 'Olio EVO', unit: 'L', price: 8.90 }
      ]
    },
    'FOODEX': {
      schedule: ['Martedì', 'Giovedì'],
      products: [
        { name: 'Riso bianco', unit: 'kg', price: 2.80 },
        { name: 'Riso integrale', unit: 'kg', price: 3.20 },
        { name: 'Riso venere', unit: 'kg', price: 4.50 },
        { name: 'Zenzero (Thailandia)', unit: 'kg', price: 8.90 },
        { name: 'Cipolla crispy', unit: 'kg', price: 12.00 },
        { name: 'Alga nori', unit: 'pz', price: 2.50 },
        { name: 'Semi di sesamo bianchi', unit: 'kg', price: 7.80 },
        { name: 'Semi di sesamo neri', unit: 'kg', price: 9.20 },
        { name: 'Salsa di soia', unit: 'L', price: 4.50 },
        { name: 'Teriyaki', unit: 'L', price: 8.90 },
        { name: 'Mayo giappo', unit: 'L', price: 6.50 },
        { name: 'Condimento riso (shiragiku)', unit: 'L', price: 12.00 },
        { name: 'Olio di sesamo', unit: 'L', price: 15.00 },
        { name: 'Edamame', unit: 'kg', price: 5.50 },
        { name: 'Goma wakame', unit: 'kg', price: 18.00 }
      ]
    },
    'MAGAZZINO': {
      schedule: ['Sabato', 'Domenica', 'Lunedì'],
      products: [
        { name: 'Shopper', unit: 'pz', price: 0.25 },
        { name: 'Tovaglioli', unit: 'pacco', price: 2.20 },
        { name: 'Bacchette', unit: 'pz', price: 0.08 }
      ]
    },
    'FORNITORI FERRARA': {
      schedule: [],
      products: [
        { name: 'Bowl Kids', unit: 'pz', price: 0.35 },
        { name: 'Bowl regular', unit: 'pz', price: 0.45 },
        { name: 'Bowl large', unit: 'pz', price: 0.55 },
        { name: 'Tappi kids', unit: 'pz', price: 0.12 },
        { name: 'Tappi regular', unit: 'pz', price: 0.15 },
        { name: 'Tappi large', unit: 'pz', price: 0.18 },
        { name: 'Cofanetto', unit: 'pz', price: 0.80 }
      ]
    },
    'CARTAPACKING': {
      schedule: ['Lunedì', 'Mercoledì'],
      products: [
        { name: 'Bicchieri', unit: 'pz', price: 0.08 }
      ]
    }
  });

  // Stati per inventario e trasferimenti
  const [inventory, setInventory] = useState({});
  const [transfers, setTransfers] = useState([]);
  const [transferItems, setTransferItems] = useState({});
  const [orders, setOrders] = useState([]);
  const [dailyUsage, setDailyUsage] = useState({});
  const [monthlyReports, setMonthlyReports] = useState([]);
  const [importHistory, setImportHistory] = useState([]);
  
  // Stato per nuovo ordine
  const [newOrder, setNewOrder] = useState({
    supplier: '',
    products: [],
    store: '',
    date: new Date().toISOString().split('T')[0]
  });

  // Stati per import
  const [activeOrderMethod, setActiveOrderMethod] = useState('manual');
  const [importedOrders, setImportedOrders] = useState([]);
  const [activeInputMethod, setActiveInputMethod] = useState('manual');
  const [emailTemplate, setEmailTemplate] = useState('');

  // Stati per utilizzo giornaliero
  const [proteinUsage, setProteinUsage] = useState({
    pollo: 0,
    salmone: 0,
    tonno: 0,
    gamberi: 0,
    veggie: 0
  });
  
  const [produceUsage, setProduceUsage] = useState({
    avocado: 0,
    mango: 0
  });
  
  const [beverageSales, setBeverageSales] = useState({
    'Vitamin Defence': 0,
    'Vitamin Relax': 0,
    'Vitamin Zero ignite': 0,
    'Vitamin Fiber': 0,
    'Coca Cola': 0,
    'Coca Cola zero': 0,
    'Sprite': 0,
    'Fanta': 0,
    'Acqua Valser frizzante': 0,
    'Acqua Maui': 0,
    'Nestea Limone': 0,
    'Nestea Pesca': 0,
    'Heineken': 0,
    'Yuzu e Citro': 0,
    'Acai berry': 0,
    'Citro': 0,
    'Goji': 0,
    'Granade': 0,
    'Acqua di cocco': 0
  });
  
  const [bowlsCount, setBowlsCount] = useState({
    kids: 0,
    regular: 0,
    large: 0
  });

  // Funzione di login
  const handleLogin = () => {
    if (username === 'maui' && password === 'poke2025') {
      setIsAuthenticated(true);
      setCurrentUser(username);
      setUsername('');
      setPassword('');
    } else {
      alert('Username o password non corretti!\n\nProva con:\nUsername: maui\nPassword: poke2025');
    }
  };
  
  const handleKeyPress = (e) => {
    if (e.key === 'Enter') {
      handleLogin();
    }
  };
  
  const handleLogout = () => {
    setIsAuthenticated(false);
    setCurrentUser('');
  };

  // Gestione trasferimenti
  const handleTransferInput = (key, value) => {
    setTransferItems(prev => ({
      ...prev,
      [key]: value
    }));
  };

  const submitTransferGrancia = () => {
    const items = [];
    Object.entries(transferItems).forEach(([key, value]) => {
      if (key.startsWith('grancia_') && value) {
        items.push({
          name: key.replace('grancia_', ''),
          quantity: parseFloat(value)
        });
      }
    });

    if (items.length === 0) {
      alert('Inserisci almeno un prodotto da trasferire');
      return;
    }

    const transfer = {
      id: Date.now(),
      fromStore: 'Lugano',
      toStore: 'Grancia',
      items: items,
      date: new Date().toLocaleDateString('it-IT'),
      status: 'pending',
      timestamp: new Date().toISOString()
    };

    setTransfers(prev => [...prev, transfer]);
    
    // Pulisci i campi di Grancia
    const newTransferItems = { ...transferItems };
    Object.keys(newTransferItems).forEach(key => {
      if (key.startsWith('grancia_')) {
        delete newTransferItems[key];
      }
    });
    setTransferItems(newTransferItems);
    
    alert('Trasferimento registrato con successo!');
  };

  const submitTransferLido = () => {
    const items = [];
    Object.entries(transferItems).forEach(([key, value]) => {
      if (key.startsWith('lido_') && value) {
        items.push({
          name: key.replace('lido_', ''),
          quantity: parseFloat(value)
        });
      }
    });

    if (items.length === 0) {
      alert('Inserisci almeno un prodotto da trasferire');
      return;
    }

    const transfer = {
      id: Date.now(),
      fromStore: 'Lugano',
      toStore: 'Lido',
      items: items,
      date: new Date().toLocaleDateString('it-IT'),
      status: 'pending',
      timestamp: new Date().toISOString()
    };

    setTransfers(prev => [...prev, transfer]);
    
    // Pulisci i campi di Lido
    const newTransferItems = { ...transferItems };
    Object.keys(newTransferItems).forEach(key => {
      if (key.startsWith('lido_')) {
        delete newTransferItems[key];
      }
    });
    setTransferItems(newTransferItems);
    
    alert('Trasferimento registrato con successo!');
  };

  const confirmTransfer = (transferId) => {
    setTransfers(prev => prev.map(t => 
      t.id === transferId ? { ...t, status: 'completed' } : t
    ));
    alert('Trasferimento confermato!');
  };

  // Parse file ordini con formato specifico per fornitore
  const parseOrderFile = (content) => {
    try {
      const lines = content.split('\n');
      let currentSupplier = '';
      let currentStore = selectedStore;
      let orderDate = new Date().toISOString().split('T')[0];
      const ordersList = [];
      const products = [];
      
      const contentLower = content.toLowerCase();
      if (contentLower.includes('foodex')) currentSupplier = 'FOODEX';
      else if (contentLower.includes('pack') || contentLower.includes('cartapacking') || contentLower.includes('ferrara')) currentSupplier = 'PACK';
      else if (contentLower.includes('transgourmet')) currentSupplier = 'TRANSGOURMET';
      else if (contentLower.includes('bianchi')) currentSupplier = 'BIANCHI';
      
      lines.forEach((line, index) => {
        const cleanLine = line.trim();
        
        if (cleanLine.toLowerCase().includes('store:')) {
          const match = cleanLine.match(/store:\s*(.+)/i);
          if (match) currentStore = match[1].trim();
        }
        if (cleanLine.toLowerCase().includes('fornitore:')) {
          const match = cleanLine.match(/fornitore:\s*(.+)/i);
          if (match) currentSupplier = match[1].trim().toUpperCase();
        }
        if (cleanLine.toLowerCase().includes('data:')) {
          const match = cleanLine.match(/data:\s*(.+)/i);
          if (match) {
            const dateStr = match[1].trim();
            const parsed = new Date(dateStr);
            if (!isNaN(parsed)) {
              orderDate = parsed.toISOString().split('T')[0];
            }
          }
        }
        
        if (index === 0 || cleanLine.toLowerCase().includes('prodotto') || cleanLine.toLowerCase().includes('codice') || cleanLine === '') {
          return;
        }
        
        const separator = cleanLine.includes('\t') ? '\t' : cleanLine.includes('|') ? '|' : ',';
        const columns = cleanLine.split(separator).map(col => col.trim());
        
        if (columns.length >= 3) {
          let productName = '';
          let quantity = 0;
          let price = 0;
          let unit = 'pz';
          
          if (currentSupplier === 'FOODEX' && columns.length >= 8) {
            productName = columns[0];
            price = parseFloat(columns[4]) || 0;
            quantity = parseFloat(columns[7]) || 0;
          } else if (currentSupplier === 'PACK' && columns.length >= 6) {
            productName = columns[0];
            price = parseFloat(columns[2]) || 0;
            quantity = parseFloat(columns[5]) || 0;
          } else if (currentSupplier === 'TRANSGOURMET' && columns.length >= 7) {
            productName = columns[0];
            price = parseFloat(columns[3]) || 0;
            quantity = parseFloat(columns[6]) || 0;
          } else if (currentSupplier === 'BIANCHI' && columns.length >= 7) {
            productName = columns[0];
            price = parseFloat(columns[3]) || 0;
            if (columns[7] && parseFloat(columns[7]) > 0) {
              quantity = parseFloat(columns[7]);
              unit = 'kg';
            } else if (columns[6] && parseFloat(columns[6]) > 0) {
              quantity = parseFloat(columns[6]);
              unit = 'pz';
            }
          } else if (columns.length >= 2) {
            productName = columns[0];
            quantity = parseFloat(columns[1]) || 0;
            if (columns[2]) {
              price = parseFloat(columns[2]) || 0;
            }
          }
          
          if (productName && quantity > 0) {
            let actualSupplier = currentSupplier;
            if (currentSupplier === 'PACK') {
              const productLower = productName.toLowerCase();
              if (productLower.includes('bowl') || productLower.includes('tappi') || productLower.includes('cofanetto')) {
                actualSupplier = 'FORNITORI FERRARA';
              } else if (productLower.includes('bicchier')) {
                actualSupplier = 'CARTAPACKING';
              } else if (productLower.includes('acqua maui') || productLower.includes('shopper') || productLower.includes('tovaglioli') || productLower.includes('bacchette')) {
                actualSupplier = 'MAGAZZINO';
              }
            }
            
            const supplierData = suppliers[actualSupplier];
            if (supplierData) {
              let foundProduct = supplierData.products.find(p => {
                const pNameLower = p.name.toLowerCase();
                const searchNameLower = productName.toLowerCase();
                return pNameLower === searchNameLower || 
                       pNameLower.includes(searchNameLower) ||
                       searchNameLower.includes(pNameLower);
              });
              
              if (foundProduct) {
                const finalPrice = price > 0 ? price : foundProduct.price;
                products.push({
                  ...foundProduct,
                  price: finalPrice,
                  quantity: quantity,
                  unit: unit || foundProduct.unit,
                  supplier: actualSupplier
                });
              } else {
                if (price > 0) {
                  products.push({
                    name: productName,
                    price: price,
                    quantity: quantity,
                    unit: unit,
                    supplier: actualSupplier
                  });
                }
              }
            }
          }
        }
      });
      
      const productsBySupplier = {};
      products.forEach(product => {
        if (!productsBySupplier[product.supplier]) {
          productsBySupplier[product.supplier] = [];
        }
        productsBySupplier[product.supplier].push(product);
      });
      
      Object.entries(productsBySupplier).forEach(([supplier, prods]) => {
        if (prods.length > 0) {
          ordersList.push({
            supplier,
            products: prods,
            store: currentStore,
            date: orderDate
          });
        }
      });
      
      return ordersList;
    } catch (error) {
      console.error('Errore parsing file:', error);
      alert('Errore nel parsing del file. Verifica il formato.');
      return [];
    }
  };

  // Genera template ordini
  const generateOrderTemplate = () => {
    const template = `TEMPLATE ORDINI - MAUI POKE
Data: ${new Date().toLocaleDateString('it-IT')}
Store: ${selectedStore}

=== FORMATO FOODEX ===
Colonne: Prodotto | Codice | Pz Min | Min Ordine | Prezzo | Magazzino | Val Mag | Qtà Ordinare | Costo
Esempio:
Riso bianco	RB001	1	10	2.80	5	14.00	20	56.00
Riso venere	RV002	1	5	4.50	2	9.00	10	45.00

=== FORMATO PACK ===
Colonne: Prodotto | Pz Min | Prezzo | Magazzino | Val Mag | Qtà Ordinare | N° Pezzi | Costo
Esempio:
Bowl regular	50	0.45	100	45.00	200	200	90.00

=== FORMATO TRANSGOURMET ===
Colonne: Prodotto | Pz Min | Codice | Prezzo | Magazzino | Val Mag | Qtà Ordinare | N° Pezzi | Costo
Esempio:
Avocado	1	AV123	2.50	20	50.00	50	50	125.00

=== FORMATO BIANCHI ===
Colonne: Prodotto | Pz Min | Codice | Prezzo | Magazzino | Val Mag | Qtà Pz | Qtà Kg | Costo
Esempio:
POLLO	1	PL789	12.50	10	125.00	0	15	187.50`;
    
    const blob = new Blob([template], { type: 'text/plain' });
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = `template_ordini_${selectedStore}_${new Date().toISOString().split('T')[0]}.txt`;
    a.click();
  };

  // Genera template utilizzo
  const generateUsageTemplate = () => {
    const template = `TEMPLATE UTILIZZO GIORNALIERO
Store: ${selectedStore}
Data: ${new Date().toLocaleDateString('it-IT')}

PROTEINE (Scoop):
Pollo:
Salmone:
Tonno:
Gamberi:
Veggie:

BOWL VENDUTE:
Kids:
Regular:
Large:

FRUTTA E VERDURA:
Avocado:
Mango:

BEVANDE:
Vitamin Defence:
Vitamin Relax:
Vitamin Zero ignite:
Vitamin Fiber:
Coca Cola:
Coca Cola Zero:
Sprite:
Fanta:
Acqua Valser frizzante:
Acqua Maui:
Nestea Limone:
Nestea Pesca:
Heineken:
Yuzu e Citro:
Acai berry:
Citro:
Goji:
Granade:
Acqua di cocco:`;
    
    const blob = new Blob([template], { type: 'text/plain' });
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = `template_utilizzo_${selectedStore}_${new Date().toISOString().split('T')[0]}.txt`;
    a.click();
  };

  // Handle file upload ordini
  const handleOrderFileUpload = (event) => {
    const file = event.target.files[0];
    if (file) {
      const reader = new FileReader();
      reader.onload = (e) => {
        const content = e.target.result;
        const ordersList = parseOrderFile(content);
        if (ordersList.length > 0) {
          setImportedOrders(ordersList);
          alert(`Importati ${ordersList.length} ordini. Verifica e conferma.`);
        }
      };
      reader.readAsText(file);
    }
  };

  // Handle paste da Numbers
  const handlePasteData = (e) => {
    const pastedData = e.clipboardData.getData('text');
    const ordersList = parseOrderFile(pastedData);
    if (ordersList.length > 0) {
      setImportedOrders(ordersList);
      alert(`Importati ${ordersList.length} ordini. Verifica e conferma.`);
    }
  };

  // Conferma ordini importati
  const confirmImportedOrders = () => {
    importedOrders.forEach(orderData => {
      const orderTotal = orderData.products.reduce((sum, p) => sum + (p.price * p.quantity), 0);
      const order = {
        ...orderData,
        id: Date.now() + Math.random(),
        total: orderTotal,
        timestamp: new Date().toISOString()
      };
      setOrders(prev => [...prev, order]);
    });
    setImportedOrders([]);
    alert('Ordini registrati con successo!');
  };

  // Parse email/text utilizzo
  const parseEmailData = (text) => {
    try {
      const lines = text.split('\n');
      const data = {
        proteins: {},
        bowls: {},
        produce: {},
        beverages: {}
      };
      
      let currentSection = '';
      
      lines.forEach(line => {
        const cleanLine = line.trim().toLowerCase();
        
        if (cleanLine.includes('proteine') || cleanLine.includes('scoop')) {
          currentSection = 'proteins';
        } else if (cleanLine.includes('bowl')) {
          currentSection = 'bowls';
        } else if (cleanLine.includes('frutta') || cleanLine.includes('verdura')) {
          currentSection = 'produce';
        } else if (cleanLine.includes('bevande')) {
          currentSection = 'beverages';
        }
        
        const match = line.match(/(.+?):\s*(\d+)/);
        if (match) {
          const [, item, value] = match;
          const itemClean = item.trim().toLowerCase();
          const numValue = parseInt(value);
          
          if (currentSection === 'proteins') {
            if (itemClean.includes('pollo')) data.proteins.pollo = numValue;
            if (itemClean.includes('salmone')) data.proteins.salmone = numValue;
            if (itemClean.includes('tonno')) data.proteins.tonno = numValue;
            if (itemClean.includes('gamberi')) data.proteins.gamberi = numValue;
            if (itemClean.includes('veggie')) data.proteins.veggie = numValue;
          } else if (currentSection === 'bowls') {
            if (itemClean.includes('kids')) data.bowls.kids = numValue;
            if (itemClean.includes('regular')) data.bowls.regular = numValue;
            if (itemClean.includes('large')) data.bowls.large = numValue;
          } else if (currentSection === 'produce') {
            if (itemClean.includes('avocado')) data.produce.avocado = numValue;
            if (itemClean.includes('mango')) data.produce.mango = numValue;
          } else if (currentSection === 'beverages') {
            if (itemClean.includes('vitamin defence')) data.beverages['Vitamin Defence'] = numValue;
            if (itemClean.includes('vitamin relax')) data.beverages['Vitamin Relax'] = numValue;
            if (itemClean.includes('vitamin zero ignite')) data.beverages['Vitamin Zero ignite'] = numValue;
            if (itemClean.includes('vitamin fiber')) data.beverages['Vitamin Fiber'] = numValue;
            if (itemClean.includes('coca cola zero')) data.beverages['Coca Cola zero'] = numValue;
            else if (itemClean.includes('coca cola')) data.beverages['Coca Cola'] = numValue;
            if (itemClean.includes('sprite')) data.beverages['Sprite'] = numValue;
            if (itemClean.includes('fanta')) data.beverages['Fanta'] = numValue;
            if (itemClean.includes('acqua valser')) data.beverages['Acqua Valser frizzante'] = numValue;
            if (itemClean.includes('acqua maui')) data.beverages['Acqua Maui'] = numValue;
            if (itemClean.includes('nestea limone')) data.beverages['Nestea Limone'] = numValue;
            if (itemClean.includes('nestea pesca')) data.beverages['Nestea Pesca'] = numValue;
            if (itemClean.includes('heineken')) data.beverages['Heineken'] = numValue;
            if (itemClean.includes('yuzu')) data.beverages['Yuzu e Citro'] = numValue;
            if (itemClean.includes('acai')) data.beverages['Acai berry'] = numValue;
            if (itemClean.includes('citro') && !itemClean.includes('yuzu')) data.beverages['Citro'] = numValue;
            if (itemClean.includes('goji')) data.beverages['Goji'] = numValue;
            if (itemClean.includes('granade')) data.beverages['Granade'] = numValue;
            if (itemClean.includes('acqua di cocco') || itemClean.includes('cocco')) data.beverages['Acqua di cocco'] = numValue;
          }
        }
      });
      
      return data;
    } catch (error) {
      alert('Errore nel parsing dei dati');
      return null;
    }
  };

  // Handle file upload utilizzo
  const handleUsageFileUpload = (event) => {
    const file = event.target.files[0];
    if (file) {
      const reader = new FileReader();
      reader.onload = (e) => {
        const content = e.target.result;
        const data = parseEmailData(content);
        if (data) {
          setProteinUsage(prev => ({ ...prev, ...data.proteins }));
          setBowlsCount(prev => ({ ...prev, ...data.bowls }));
          setProduceUsage(prev => ({ ...prev, ...data.produce }));
          setBeverageSales(prev => ({ ...prev, ...data.beverages }));
          alert('File importato! Verifica i valori e premi "Registra Utilizzo"');
          setActiveInputMethod('manual');
        }
      };
      reader.readAsText(file);
    }
  };

  // Handle import email
  const handleEmailImport = () => {
    const data = parseEmailData(emailTemplate);
    if (data) {
      setProteinUsage(prev => ({ ...prev, ...data.proteins }));
      setBowlsCount(prev => ({ ...prev, ...data.bowls }));
      setProduceUsage(prev => ({ ...prev, ...data.produce }));
      setBeverageSales(prev => ({ ...prev, ...data.beverages }));
      alert('Dati importati! Verifica e registra.');
      setActiveInputMethod('manual');
      setEmailTemplate('');
    }
  };

  // Registra utilizzo giornaliero
  const updateUsage = () => {
    const usage = {
      store: selectedStore,
      date: new Date().toISOString().split('T')[0],
      proteins: { ...proteinUsage },
      produce: { ...produceUsage },
      beverages: { ...beverageSales },
      bowls: { ...bowlsCount },
      timestamp: new Date().toISOString()
    };
    
    setDailyUsage(prev => ({
      ...prev,
      [selectedStore]: [...(prev[selectedStore] || []), usage]
    }));
    
    setImportHistory(prev => [...prev, {
      date: usage.date,
      store: selectedStore,
      method: activeInputMethod,
      time: new Date().toLocaleTimeString('it-IT')
    }]);
    
    setProteinUsage({ pollo: 0, salmone: 0, tonno: 0, gamberi: 0, veggie: 0 });
    setProduceUsage({ avocado: 0, mango: 0 });
    setBeverageSales({
      'Vitamin Defence': 0,
      'Vitamin Relax': 0,
      'Vitamin Zero ignite': 0,
      'Vitamin Fiber': 0,
      'Coca Cola': 0,
      'Coca Cola zero': 0,
      'Sprite': 0,
      'Fanta': 0,
      'Acqua Valser frizzante': 0,
      'Acqua Maui': 0,
      'Nestea Limone': 0,
      'Nestea Pesca': 0,
      'Heineken': 0,
      'Yuzu e Citro': 0,
      'Acai berry': 0,
      'Citro': 0,
      'Goji': 0,
      'Granade': 0,
      'Acqua di cocco': 0
    });
    setBowlsCount({ kids: 0, regular: 0, large: 0 });
    
    alert('Utilizzo giornaliero registrato!');
  };

  // Gestione ordini
  const addProductToOrder = (product) => {
    setNewOrder(prev => ({
      ...prev,
      products: [...prev.products, { ...product, quantity: 1 }]
    }));
  };

  const updateOrderQuantity = (index, quantity) => {
    setNewOrder(prev => ({
      ...prev,
      products: prev.products.map((p, i) => 
        i === index ? { ...p, quantity: parseInt(quantity) || 0 } : p
      )
    }));
  };

  const submitOrder = () => {
    if (!newOrder.supplier || !newOrder.store || newOrder.products.length === 0) {
      alert('Completa tutti i campi');
      return;
    }
    
    const orderTotal = newOrder.products.reduce((sum, p) => sum + (p.price * p.quantity), 0);
    
    const order = {
      ...newOrder,
      id: Date.now(),
      total: orderTotal,
      timestamp: new Date().toISOString()
    };
    
    setOrders(prev => [...prev, order]);
    
    setNewOrder({
      supplier: '',
      products: [],
      store: '',
      date: new Date().toISOString().split('T')[0]
    });
    
    alert('Ordine registrato!');
  };

  // Calcolo Food Cost
  const calculateFoodCost = (store) => {
    const storeOrders = orders.filter(o => o.store === store);
    const revenue = 15000;
    const totalCost = storeOrders.reduce((sum, o) => sum + o.total, 0);
    const percentage = revenue > 0 ? (totalCost / revenue * 100) : 0;
    
    return {
      totalCost,
      revenue,
      percentage: percentage.toFixed(2)
    };
  };

  // Generate monthly report
  const generateMonthlyReport = () => {
    const report = {
      month: new Date().toLocaleDateString('it-IT', { month: 'long', year: 'numeric' }),
      stores: stores.map(store => ({
        name: store,
        foodCost: calculateFoodCost(store),
        ordersCount: orders.filter(o => o.store === store).length,
        transfersReceived: transfers.filter(t => t.toStore === store).length,
        transfersSent: transfers.filter(t => t.fromStore === store).length
      }))
    };
    
    setMonthlyReports(prev => [...prev, report]);
    alert('Report mensile generato!');
  };

  // SCHERMATA LOGIN
  if (!isAuthenticated) {
    return (
      <div className="min-h-screen bg-gradient-to-br from-blue-500 to-purple-600 flex items-center justify-center p-4">
        <div className="bg-white rounded-2xl shadow-2xl p-8 w-full max-w-md">
          <div className="text-center mb-8">
            <div className="inline-flex items-center justify-center w-20 h-20 bg-blue-100 rounded-full mb-4">
              <Lock className="h-10 w-10 text-blue-600" />
            </div>
            <h1 className="text-3xl font-bold text-gray-900 mb-2">Maui Poke</h1>
            <p className="text-gray-600">Food Cost Manager</p>
          </div>
          
          <div>
            <div className="mb-4">
              <label className="block text-sm font-medium text-gray-700 mb-2">Username</label>
              <input
                type="text"
                value={username}
                onChange={(e) => setUsername(e.target.value)}
                onKeyPress={handleKeyPress}
                className="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
                placeholder="Inserisci username"
              />
            </div>
            
            <div className="mb-6">
              <label className="block text-sm font-medium text-gray-700 mb-2">Password</label>
              <input
                type="password"
                value={password}
                onChange={(e) => setPassword(e.target.value)}
                onKeyPress={handleKeyPress}
                className="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
                placeholder="Inserisci password"
              />
            </div>
            
            <button
              type="button"
              onClick={handleLogin}
              className="w-full py-3 px-4 bg-blue-600 hover:bg-blue-700 text-white font-medium rounded-lg transition duration-200 cursor-pointer"
            >
              Accedi
            </button>
          </div>
          
          <div className="mt-6 p-3 bg-yellow-50 rounded-lg">
            <p className="text-xs text-yellow-800 text-center font-semibold">🔐 Credenziali Demo</p>
            <div className="text-xs text-yellow-700 text-center mt-1">
              <p>Username: <span className="font-mono font-bold">maui</span></p>
              <p>Password: <span className="font-mono font-bold">poke2025</span></p>
            </div>
          </div>
        </div>
      </div>
    );
  }

  // APP PRINCIPALE
  return (
    <div className="min-h-screen bg-gray-100">
      <header className="bg-white shadow-sm border-b">
        <div className="px-4 lg:px-8 py-4">
          <div className="flex justify-between items-center">
            <div className="flex items-center space-x-4">
              <h1 className="text-2xl font-bold text-gray-900">Maui Poke</h1>
              <select
                value={selectedStore}
                onChange={(e) => setSelectedStore(e.target.value)}
                className="px-3 py-1 border rounded-lg text-sm"
              >
                {stores.map(store => (
                  <option key={store} value={store}>{store}</option>
                ))}
              </select>
            </div>
            <div className="flex items-center space-x-4">
              <span className="text-sm text-blue-600">👤 {currentUser}</span>
              <span className="text-sm text-gray-500">{currentDate}</span>
              <button
                onClick={handleLogout}
                className="px-3 py-1 bg-red-500 text-white rounded hover:bg-red-600"
              >
                Esci
              </button>
            </div>
          </div>
        </div>
      </header>

      <nav className="bg-white shadow-sm">
        <div className="px-4 lg:px-8">
          <div className="flex space-x-6 overflow-x-auto">
            {[
              { id: 'dashboard', label: 'Dashboard', icon: <Home className="h-4 w-4" /> },
              { id: 'orders', label: 'Ordini', icon: <Package className="h-4 w-4" /> },
              { id: 'transfers', label: 'Trasferimenti', icon: <Truck className="h-4 w-4" /> },
              { id: 'inventory', label: 'Magazzino', icon: <FileText className="h-4 w-4" /> },
              { id: 'reports', label: 'Report', icon: <BarChart3 className="h-4 w-4" /> }
            ].map(tab => (
              <button
                key={tab.id}
                onClick={() => setActiveTab(tab.id)}
                className={`py-4 px-1 border-b-2 whitespace-nowrap flex items-center space-x-2 ${
                  activeTab === tab.id
                    ? 'border-blue-500 text-blue-600'
                    : 'border-transparent text-gray-500 hover:text-gray-700'
                }`}
              >
                {tab.icon}
                <span>{tab.label}</span>
              </button>
            ))}
          </div>
        </div>
      </nav>

      <main className="p-6">
        {/* DASHBOARD */}
        {activeTab === 'dashboard' && (
          <div className="space-y-6">
            <div className="grid grid-cols-1 md:grid-cols-4 gap-4">
              <div className="bg-white rounded-lg shadow p-6">
                <div className="flex items-center justify-between mb-2">
                  <p className="text-sm text-gray-600">Food Cost %</p>
                  <DollarSign className="h-5 w-5 text-blue-600" />
                </div>
                <p className="text-2xl font-bold text-blue-600">{calculateFoodCost(selectedStore).percentage}%</p>
                <p className="text-xs text-gray-500 mt-1">Target: &lt;35%</p>
              </div>
              <div className="bg-white rounded-lg shadow p-6">
                <div className="flex items-center justify-between mb-2">
                  <p className="text-sm text-gray-600">Ordini Oggi</p>
                  <Package className="h-5 w-5 text-green-600" />
                </div>
                <p className="text-2xl font-bold text-green-600">
                  {orders.filter(o => o.date === new Date().toISOString().split('T')[0]).length}
                </p>
              </div>
              <div className="bg-white rounded-lg shadow p-6">
                <div className="flex items-center justify-between mb-2">
                  <p className="text-sm text-gray-600">Trasferimenti</p>
                  <Truck className="h-5 w-5 text-orange-600" />
                </div>
                <p className="text-2xl font-bold text-orange-600">{transfers.length}</p>
              </div>
              <div className="bg-white rounded-lg shadow p-6">
                <div className="flex items-center justify-between mb-2">
                  <p className="text-sm text-gray-600">Store</p>
                  <Users className="h-5 w-5 text-purple-600" />
                </div>
                <p className="text-xl font-bold text-purple-600">{selectedStore}</p>
              </div>
            </div>
            
            <div className="bg-yellow-50 border-l-4 border-yellow-400 p-4">
              <div className="flex">
                <AlertCircle className="h-5 w-5 text-yellow-400" />
                <div className="ml-3">
                  <p className="text-sm text-yellow-700 font-semibold">Ordini da effettuare entro le 18:00</p>
                  <ul className="mt-2 text-sm text-yellow-600">
                    {Object.entries(suppliers).map(([supplier, data]) => {
                      const days = ['Domenica', 'Lunedì', 'Martedì', 'Mercoledì', 'Giovedì', 'Venerdì', 'Sabato'];
                      const today = days[new Date().getDay()];
                      if (data.schedule && data.schedule.includes(today)) {
                        return <li key={supplier}>• {supplier}</li>;
                      }
                      return null;
                    })}
                  </ul>
                </div>
              </div>
            </div>

            <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
              <div className="bg-white rounded-lg shadow p-6">
                <h3 className="text-lg font-semibold mb-4">Ordini Recenti</h3>
                <div className="space-y-2">
                  {orders.slice(-3).reverse().map(order => (
                    <div key={order.id} className="flex justify-between items-center p-2 bg-gray-50 rounded">
                      <div>
                        <span className="font-medium">{order.supplier}</span>
                        <span className="text-sm text-gray-500 ml-2">{order.date}</span>
                      </div>
                      <span className="font-semibold">€{order.total.toFixed(2)}</span>
                    </div>
                  ))}
                  {orders.length === 0 && (
                    <p className="text-gray-500">Nessun ordine registrato</p>
                  )}
                </div>
              </div>

              <div className="bg-white rounded-lg shadow p-6">
                <h3 className="text-lg font-semibold mb-4">Utilizzo Recente</h3>
                <div className="space-y-2">
                  {dailyUsage[selectedStore] && dailyUsage[selectedStore].slice(-3).reverse().map((usage, idx) => (
                    <div key={idx} className="p-2 bg-gray-50 rounded">
                      <div className="flex justify-between items-center">
                        <span className="text-sm font-medium">{usage.date}</span>
                        <span className="text-xs text-gray-500">{usage.store}</span>
                      </div>
                      <div className="text-xs text-gray-600 mt-1">
                        Bowl: {usage.bowls.kids + usage.bowls.regular + usage.bowls.large} totali
                      </div>
                    </div>
                  ))}
                  {(!dailyUsage[selectedStore] || dailyUsage[selectedStore].length === 0) && (
                    <p className="text-gray-500">Nessun utilizzo registrato</p>
                  )}
                </div>
              </div>
            </div>
          </div>
        )}

        {/* ORDINI */}
        {activeTab === 'orders' && (
          <div className="space-y-6">
            <div className="bg-white rounded-lg shadow p-6">
              <h2 className="text-xl font-bold mb-4">Gestione Ordini</h2>
              
              <div className="flex space-x-4 mb-4">
                <button
                  onClick={() => setActiveOrderMethod('manual')}
                  className={`px-4 py-2 rounded-lg ${
                    activeOrderMethod === 'manual' ? 'bg-blue-600 text-white' : 'bg-gray-200'
                  }`}
                >
                  Manuale
                </button>
                <button
                  onClick={() => setActiveOrderMethod('file')}
                  className={`px-4 py-2 rounded-lg ${
                    activeOrderMethod === 'file' ? 'bg-blue-600 text-white' : 'bg-gray-200'
                  }`}
                >
                  Import File
                </button>
                <button
                  onClick={generateOrderTemplate}
                  className="px-4 py-2 bg-green-600 text-white rounded-lg"
                >
                  <Download className="inline h-4 w-4 mr-2" />
                  Template
                </button>
              </div>

              {activeOrderMethod === 'file' && (
                <div>
                  <div className="border-2 border-dashed border-gray-300 rounded-lg p-8 text-center mb-4">
                    <Upload className="h-12 w-12 text-gray-400 mx-auto mb-4" />
                    <input
                      type="file"
                      accept=".txt,.csv,.xlsx"
                      onChange={handleOrderFileUpload}
                      className="hidden"
                      id="orderFile"
                    />
                    <label
                      htmlFor="orderFile"
                      className="px-4 py-2 bg-blue-600 text-white rounded-lg cursor-pointer"
                    >
                      Seleziona File
                    </label>
                  </div>
                  
                  <textarea
                    onPaste={handlePasteData}
                    placeholder="Oppure incolla da Numbers/Excel"
                    className="w-full h-32 px-3 py-2 border rounded-lg"
                  />
                  
                  {importedOrders.length > 0 && (
                    <div className="mt-4 p-4 bg-blue-50 rounded">
                      <h3 className="font-semibold mb-2">Ordini da importare:</h3>
                      {importedOrders.map((order, idx) => (
                        <div key={idx} className="mb-2">
                          <span className="font-medium">{order.supplier}</span> - 
                          <span className="ml-2">{order.products.length} prodotti</span> - 
                          <span className="ml-2">€{order.products.reduce((sum, p) => sum + (p.price * p.quantity), 0).toFixed(2)}</span>
                        </div>
                      ))}
                      <button
                        onClick={confirmImportedOrders}
                        className="mt-2 px-4 py-2 bg-green-600 text-white rounded"
                      >
                        Conferma Import
                      </button>
                    </div>
                  )}
                </div>
              )}

              {activeOrderMethod === 'manual' && (
                <div>
                  <div className="grid grid-cols-3 gap-4 mb-4">
                    <select
                      value={newOrder.store}
                      onChange={(e) => setNewOrder(prev => ({ ...prev, store: e.target.value }))}
                      className="px-3 py-2 border rounded-lg"
                    >
                      <option value="">Seleziona Store</option>
                      {stores.map(store => (
                        <option key={store} value={store}>{store}</option>
                      ))}
                    </select>
                    
                    <select
                      value={newOrder.supplier}
                      onChange={(e) => setNewOrder(prev => ({ ...prev, supplier: e.target.value }))}
                      className="px-3 py-2 border rounded-lg"
                    >
                      <option value="">Seleziona Fornitore</option>
                      {Object.keys(suppliers).map(supplier => (
                        <option key={supplier} value={supplier}>{supplier}</option>
                      ))}
                    </select>
                    
                    <input
                      type="date"
                      value={newOrder.date}
                      onChange={(e) => setNewOrder(prev => ({ ...prev, date: e.target.value }))}
                      className="px-3 py-2 border rounded-lg"
                    />
                  </div>
                  
                  {newOrder.supplier && (
                    <div className="border rounded-lg p-4 mb-4">
                      <h3 className="font-semibold mb-3">Prodotti {newOrder.supplier}</h3>
                      <div className="grid grid-cols-2 lg:grid-cols-3 gap-2">
                        {suppliers[newOrder.supplier].products.map(product => (
                          <button
                            key={product.name}
                            onClick={() => addProductToOrder(product)}
                            className="px-3 py-2 bg-gray-50 hover:bg-blue-50 rounded text-left"
                          >
                            <span className="text-sm">{product.name}</span>
                            <span className="text-xs text-gray-500 ml-2">€{product.price}/{product.unit}</span>
                          </button>
                        ))}
                      </div>
                    </div>
                  )}
                  
                  {newOrder.products.length > 0 && (
                    <div className="border rounded-lg p-4 mb-4">
                      <h3 className="font-semibold mb-3">Prodotti Selezionati</h3>
                      {newOrder.products.map((product, index) => (
                        <div key={index} className="flex items-center justify-between p-2 bg-gray-50 rounded mb-2">
                          <span>{product.name}</span>
                          <div className="flex items-center space-x-2">
                            <input
                              type="number"
                              value={product.quantity}
                              onChange={(e) => updateOrderQuantity(index, e.target.value)}
                              className="w-20 px-2 py-1 border rounded"
                            />
                            <span>{product.unit}</span>
                            <span className="font-semibold">€{(product.price * product.quantity).toFixed(2)}</span>
                            <button
                              onClick={() => setNewOrder(prev => ({
                                ...prev,
                                products: prev.products.filter((_, i) => i !== index)
                              }))}
                              className="text-red-500"
                            >
                              <X className="h-4 w-4" />
                            </button>
                          </div>
                        </div>
                      ))}
                      <div className="text-right font-bold">
                        Totale: €{newOrder.products.reduce((sum, p) => sum + (p.price * p.quantity), 0).toFixed(2)}
                      </div>
                    </div>
                  )}
                  
                  <button
                    onClick={submitOrder}
                    disabled={!newOrder.supplier || !newOrder.store || newOrder.products.length === 0}
                    className="px-6 py-2 bg-blue-600 text-white rounded-lg disabled:bg-gray-300"
                  >
                    Registra Ordine
                  </button>
                </div>
              )}
            </div>

            <div className="bg-white rounded-lg shadow p-6">
              <h2 className="text-xl font-bold mb-4">Ordini Recenti</h2>
              <div className="overflow-x-auto">
                <table className="min-w-full">
                  <thead className="bg-gray-50">
                    <tr>
                      <th className="px-6 py-3 text-left text-xs font-medium text-gray-500">Data</th>
                      <th className="px-6 py-3 text-left text-xs font-medium text-gray-500">Store</th>
                      <th className="px-6 py-3 text-left text-xs font-medium text-gray-500">Fornitore</th>
                      <th className="px-6 py-3 text-left text-xs font-medium text-gray-500">Prodotti</th>
                      <th className="px-6 py-3 text-left text-xs font-medium text-gray-500">Totale</th>
                    </tr>
                  </thead>
                  <tbody className="divide-y divide-gray-200">
                    {orders.slice(-10).reverse().map(order => (
                      <tr key={order.id}>
                        <td className="px-6 py-4 text-sm">{order.date}</td>
                        <td className="px-6 py-4 text-sm">{order.store}</td>
                        <td className="px-6 py-4 text-sm">{order.supplier}</td>
                        <td className="px-6 py-4 text-sm">{order.products.length} articoli</td>
                        <td className="px-6 py-4 text-sm font-semibold">€{order.total.toFixed(2)}</td>
                      </tr>
                    ))}
                  </tbody>
                </table>
              </div>
            </div>
          </div>
        )}

        {/* TRASFERIMENTI */}
        {activeTab === 'transfers' && (
          <div className="space-y-6">
            <div className="bg-white rounded-lg shadow p-6">
              <h2 className="text-xl font-bold mb-4">Trasferimento Lugano → Grancia</h2>
              <div className="grid grid-cols-1 md:grid-cols-2 gap-3">
                {['Riso bianco', 'Riso venere', 'Riso integrale', 'Cous Cous', 'Salmone a tranci', 'Pollo', 'Gamberi'].map(item => (
                  <div key={item} className="flex items-center justify-between p-2 bg-gray-50 rounded">
                    <span>{item}</span>
                    <input
                      type="number"
                      step="0.1"
                      placeholder="Qtà"
                      value={transferItems[`grancia_${item}`] || ''}
                      onChange={(e) => handleTransferInput(`grancia_${item}`, e.target.value)}
                      className="w-20 px-2 py-1 border rounded"
                    />
                  </div>
                ))}
              </div>
              <button 
                type="button"
                onClick={submitTransferGrancia}
                className="mt-4 px-6 py-2 bg-green-600 text-white rounded-lg hover:bg-green-700 cursor-pointer"
              >
                Registra Trasferimento
              </button>
            </div>

            <div className="bg-white rounded-lg shadow p-6">
              <h2 className="text-xl font-bold mb-4">Trasferimento Lugano → Lido</h2>
              <div className="grid grid-cols-1 md:grid-cols-2 gap-3">
                {['Riso bianco', 'Riso venere', 'Riso integrale', 'Cous Cous', 'Salmone a tranci', 'Pollo', 'Gamberi', 'Salsa maui', 'Cetrioli', 'Cipolla rossa', 'Carote', 'Cavolo rosso'].map(item => (
                  <div key={item} className="flex items-center justify-between p-2 bg-gray-50 rounded">
                    <span>{item}</span>
                    <input
                      type="number"
                      step="0.1"
                      placeholder="Qtà"
                      value={transferItems[`lido_${item}`] || ''}
                      onChange={(e) => handleTransferInput(`lido_${item}`, e.target.value)}
                      className="w-20 px-2 py-1 border rounded"
                    />
                  </div>
                ))}
              </div>
              <button 
                type="button"
                onClick={submitTransferLido}
                className="mt-4 px-6 py-2 bg-green-600 text-white rounded-lg hover:bg-green-700 cursor-pointer"
              >
                Registra Trasferimento
              </button>
            </div>

            <div className="bg-white rounded-lg shadow p-6">
              <h2 className="text-xl font-bold mb-4">Trasferimenti Recenti</h2>
              <div className="space-y-2">
                {transfers.length === 0 ? (
                  <p className="text-gray-500">Nessun trasferimento registrato</p>
                ) : (
                  transfers.slice(-5).reverse().map((transfer) => (
                    <div key={transfer.id} className="p-3 bg-gray-50 rounded">
                      <div className="flex justify-between items-center">
                        <div>
                          <span className="font-semibold">{transfer.fromStore} → {transfer.toStore}</span>
                          <span className="ml-3 text-sm text-gray-500">{transfer.date}</span>
                        </div>
                        {transfer.status === 'pending' ? (
                          <button
                            type="button"
                            onClick={() => confirmTransfer(transfer.id)}
                            className="px-3 py-1 bg-blue-600 text-white text-sm rounded hover:bg-blue-700 cursor-pointer"
                          >
                            Conferma
                          </button>
                        ) : (
                          <span className="px-3 py-1 bg-green-100 text-green-800 text-sm rounded">
                            Completato
                          </span>
                        )}
                      </div>
                      <div className="mt-2 text-sm text-gray-600">
                        {transfer.items.map((item, idx) => (
                          <span key={idx}>
                            {item.name}: {item.quantity}
                            {idx < transfer.items.length - 1 ? ', ' : ''}
                          </span>
                        ))}
                      </div>
                    </div>
                  ))
                )}
              </div>
            </div>
          </div>
        )}

        {/* MAGAZZINO */}
        {activeTab === 'inventory' && (
          <div className="space-y-6">
            <div className="bg-white rounded-lg shadow p-6">
              <h2 className="text-xl font-bold mb-4">Utilizzo Giornaliero</h2>
              
              <div className="flex space-x-4 mb-4">
                <button
                  onClick={() => setActiveInputMethod('manual')}
                  className={`px-4 py-2 rounded-lg ${
                    activeInputMethod === 'manual' ? 'bg-blue-600 text-white' : 'bg-gray-200'
                  }`}
                >
                  Manuale
                </button>
                <button
                  onClick={() => setActiveInputMethod('excel')}
                  className={`px-4 py-2 rounded-lg ${
                    activeInputMethod === 'excel' ? 'bg-blue-600 text-white' : 'bg-gray-200'
                  }`}
                >
                  Import Excel
                </button>
                <button
                  onClick={() => setActiveInputMethod('email')}
                  className={`px-4 py-2 rounded-lg ${
                    activeInputMethod === 'email' ? 'bg-blue-600 text-white' : 'bg-gray-200'
                  }`}
                >
                  Import Email
                </button>
                <button
                  onClick={generateUsageTemplate}
                  className="px-4 py-2 bg-green-600 text-white rounded-lg"
                >
                  <Download className="inline h-4 w-4 mr-2" />
                  Template
                </button>
              </div>

              {activeInputMethod === 'excel' && (
                <div className="border-2 border-dashed border-gray-300 rounded-lg p-8 text-center">
                  <input
                    type="file"
                    accept=".txt,.csv,.xlsx"
                    onChange={handleUsageFileUpload}
                    className="hidden"
                    id="usageFile"
                  />
                  <label
                    htmlFor="usageFile"
                    className="px-4 py-2 bg-blue-600 text-white rounded-lg cursor-pointer"
                  >
                    Seleziona File
                  </label>
                </div>
              )}

              {activeInputMethod === 'email' && (
                <div>
                  <textarea
                    value={emailTemplate}
                    onChange={(e) => setEmailTemplate(e.target.value)}
                    placeholder="Incolla qui il contenuto dell'email..."
                    className="w-full h-64 px-3 py-2 border rounded-lg"
                  />
                  <button
                    onClick={handleEmailImport}
                    className="mt-4 px-6 py-2 bg-blue-600 text-white rounded-lg"
                  >
                    Importa Dati
                  </button>
                </div>
              )}

              {activeInputMethod === 'manual' && (
                <div>
                  <h3 className="font-semibold mb-3">Bowl Vendute</h3>
                  <div className="grid grid-cols-3 gap-3 mb-4">
                    {Object.keys(bowlsCount).map(size => (
                      <div key={size}>
                        <label className="text-sm text-gray-600 capitalize">{size}</label>
                        <input
                          type="number"
                          value={bowlsCount[size]}
                          onChange={(e) => setBowlsCount(prev => ({
                            ...prev,
                            [size]: parseInt(e.target.value) || 0
                          }))}
                          className="w-full px-3 py-2 border rounded-lg"
                        />
                      </div>
                    ))}
                  </div>

                  <h3 className="font-semibold mb-3">Proteine (Scoop)</h3>
                  <div className="grid grid-cols-2 md:grid-cols-5 gap-3 mb-4">
                    {Object.keys(proteinUsage).map(protein => (
                      <div key={protein}>
                        <label className="text-sm text-gray-600 capitalize">{protein}</label>
                        <input
                          type="number"
                          value={proteinUsage[protein]}
                          onChange={(e) => setProteinUsage(prev => ({
                            ...prev,
                            [protein]: parseInt(e.target.value) || 0
                          }))}
                          className="w-full px-3 py-2 border rounded-lg"
                        />
                      </div>
                    ))}
                  </div>

                  <h3 className="font-semibold mb-3">Frutta e Verdura</h3>
                  <div className="grid grid-cols-2 gap-3 mb-4">
                    {Object.keys(produceUsage).map(item => (
                      <div key={item}>
                        <label className="text-sm text-gray-600 capitalize">{item}</label>
                        <input
                          type="number"
                          value={produceUsage[item]}
                          onChange={(e) => setProduceUsage(prev => ({
                            ...prev,
                            [item]: parseInt(e.target.value) || 0
                          }))}
                          className="w-full px-3 py-2 border rounded-lg"
                        />
                      </div>
                    ))}
                  </div>

                  <h3 className="font-semibold mb-3">Bevande Vendute</h3>
                  <div className="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-3 mb-4 max-h-96 overflow-y-auto">
                    {Object.keys(beverageSales).map(beverage => (
                      <div key={beverage}>
                        <label className="text-xs text-gray-600">{beverage}</label>
                        <input
                          type="number"
                          value={beverageSales[beverage]}
                          onChange={(e) => setBeverageSales(prev => ({
                            ...prev,
                            [beverage]: parseInt(e.target.value) || 0
                          }))}
                          className="w-full px-3 py-2 border rounded-lg"
                        />
                      </div>
                    ))}
                  </div>

                  <button
                    onClick={updateUsage}
                    className="px-6 py-2 bg-blue-600 text-white rounded-lg"
                  >
                    Registra Utilizzo
                  </button>
                </div>
              )}
            </div>

            <div className="bg-white rounded-lg shadow p-6">
              <h2 className="text-xl font-bold mb-4">Storico Import</h2>
              <div className="overflow-x-auto">
                <table className="min-w-full">
                  <thead className="bg-gray-50">
                    <tr>
                      <th className="px-6 py-3 text-left text-xs font-medium text-gray-500">Data</th>
                      <th className="px-6 py-3 text-left text-xs font-medium text-gray-500">Ora</th>
                      <th className="px-6 py-3 text-left text-xs font-medium text-gray-500">Store</th>
                      <th className="px-6 py-3 text-left text-xs font-medium text-gray-500">Metodo</th>
                    </tr>
                  </thead>
                  <tbody className="divide-y divide-gray-200">
                    {importHistory.slice(-5).reverse().map((item, index) => (
                      <tr key={index}>
                        <td className="px-6 py-4 text-sm">{item.date}</td>
                        <td className="px-6 py-4 text-sm">{item.time}</td>
                        <td className="px-6 py-4 text-sm">{item.store}</td>
                        <td className="px-6 py-4 text-sm">
                          <span className={`px-2 py-1 text-xs rounded ${
                            item.method === 'manual' ? 'bg-blue-100 text-blue-800' :
                            item.method === 'excel' ? 'bg-green-100 text-green-800' :
                            'bg-purple-100 text-purple-800'
                          }`}>
                            {item.method}
                          </span>
                        </td>
                      </tr>
                    ))}
                  </tbody>
                </table>
              </div>
            </div>
          </div>
        )}

        {/* REPORT */}
        {activeTab === 'reports' && (
          <div className="space-y-6">
            <div className="bg-white rounded-lg shadow p-6">
              <div className="flex justify-between items-center mb-6">
                <h2 className="text-xl font-bold">Report e Analisi</h2>
                <button
                  onClick={generateMonthlyReport}
                  className="px-4 py-2 bg-green-600 text-white rounded-lg"
                >
                  Genera Report Mensile
                </button>
              </div>

              <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
                {stores.map(store => {
                  const fc = calculateFoodCost(store);
                  return (
                    <div key={store} className="border rounded-lg p-4">
                      <h3 className="font-semibold mb-3">{store}</h3>
                      <div className="space-y-2 text-sm">
                        <div className="flex justify-between">
                          <span>Food Cost:</span>
                          <span className={`font-semibold ${
                            parseFloat(fc.percentage) > 35 ? 'text-red-600' : 'text-green-600'
                          }`}>{fc.percentage}%</span>
                        </div>
                        <div className="flex justify-between">
                          <span>Costi:</span>
                          <span>€{fc.totalCost.toFixed(2)}</span>
                        </div>
                        <div className="flex justify-between">
                          <span>Ordini:</span>
                          <span>{orders.filter(o => o.store === store).length}</span>
                        </div>
                      </div>
                    </div>
                  );
                })}
              </div>
            </div>

            <div className="bg-blue-50 border-l-4 border-blue-400 p-4">
              <h3 className="text-lg font-semibold text-blue-800 mb-2">Suggerimenti Ottimizzazione</h3>
              <ul className="space-y-2 text-sm text-blue-700">
                <li>• Ordina in quantità maggiori da Transgourmet per sconti volume</li>
                <li>• Verifica porzioni e sprechi se il food cost supera il 35%</li>
                <li>• Consolida i trasferimenti 3 volte a settimana</li>
                <li>• Monitora avocado e mango (15% del costo totale)</li>
                <li>• Considera fornitori alternativi per proteine premium</li>
              </ul>
            </div>

            {monthlyReports.length > 0 && (
              <div className="bg-white rounded-lg shadow p-6">
                <h3 className="text-lg font-semibold mb-4">Report Archiviati</h3>
                {monthlyReports.map((report, index) => (
                  <div key={index} className="flex justify-between items-center p-3 bg-gray-50 rounded mb-2">
                    <span>{report.month}</span>
                    <button className="text-blue-600">
                      <Download className="h-4 w-4" />
                    </button>
                  </div>
                ))}
              </div>
            )}
          </div>
        )}
      </main>
    </div>
  );
};

export default MauiPokeFoodCost;
