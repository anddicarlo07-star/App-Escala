import React, { useState, useEffect } from 'react';
import { initializeApp } from 'firebase/app';
import { getFirestore, collection, getDocs, addDoc } from 'firebase/firestore';

// 1. Configuração do Firebase (Substitua pelos valores do seu projeto)
const firebaseConfig = {
  apiKey: process.env.NEXT_PUBLIC_FIREBASE_API_KEY,
  authDomain: process.env.NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN,
  projectId: process.env.NEXT_PUBLIC_FIREBASE_PROJECT_ID,
};

const app = initializeApp(firebaseConfig);
const db = getFirestore(app);

export default function AppEscala() {
  const [gerentes, setGerentes] = useState([]);

  // Exemplo de busca no Firestore
  useEffect(() => {
    const fetchData = async () => {
      const querySnapshot = await getDocs(collection(db, "gerentes"));
      setGerentes(querySnapshot.docs.map(doc => doc.data()));
    };
    fetchData();
  }, []);

  return (
    <div className="p-8 bg-gray-50 min-h-screen">
      <h1 className="text-2xl font-bold text-green-800 mb-6">
        Painel de Escalas - Leroy Merlin Morumbi
      </h1>
      
      <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
        {gerentes.map((g, index) => (
          <div key={index} className="p-4 bg-white shadow rounded-lg border-l-4 border-green-600">
            <h2 className="font-semibold">{g.nome}</h2>
            <p className="text-sm text-gray-600">{g.email}</p>
          </div>
        ))}
      </div>
      
      <button className="mt-8 px-6 py-2 bg-green-700 text-white rounded hover:bg-green-800 transition">
        Gerar Escala com IA
      </button>
    </div>
  );
}
