import React, { useState, useEffect, useCallback, useMemo } from 'react';
import { ShieldCheck, Truck, BarChart2, Bell, XCircle, Zap, RefreshCw } from 'lucide-react';
import { initializeApp } from 'firebase/app';
import { getAuth, signInAnonymously, signInWithCustomToken, onAuthStateChanged } from 'firebase/auth';
import { getFirestore, doc, setDoc, onSnapshot, collection, query, addDoc, serverTimestamp } from 'firebase/firestore';

// --- INITIAL CONFIG & GLOBAL STATE HOOK ---
const useFirebase = () => {
    const [db, setDb] = useState(null);
    const [auth, setAuth] = useState(null);
    const [userId, setUserId] = useState(null);
    const [isAuthReady, setIsAuthReady] = useState(false);

    useEffect(() => {
        // Global variables provided by the environment
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
        const firebaseConfig = typeof __firebase_config !== 'undefined' ? JSON.parse(__firebase_config) : {};
        const initialAuthToken = typeof __initial_auth_token !== 'undefined' ? __initial_auth_token : null;

        if (Object.keys(firebaseConfig).length === 0) {
            console.error("Firebase config is missing.");
            return;
        }

        const app = initializeApp(firebaseConfig);
        const firestore = getFirestore(app);
        const authInstance = getAuth(app);
        
        setDb(firestore);
        setAuth(authInstance);

        // Authentication Process
        onAuthStateChanged(authInstance, async (user) => {
            if (!user) {
                // If token is available, sign in with it
                if (initialAuthToken) {
                    try {
                        const credential = await signInWithCustomToken(authInstance, initialAuthToken);
                        setUserId(credential.user.uid);
                    } catch (error) {
                        console.error("Custom token sign-in failed, falling back to anonymous:", error);
                        await signInAnonymously(authInstance);
                        setUserId(authInstance.currentUser.uid);
                    }
                } else {
                    // Fallback to anonymous sign-in
                    await signInAnonymously(authInstance);
                    setUserId(authInstance.currentUser.uid);
                }
            } else {
                setUserId(user.uid);
            }
            setIsAuthReady(true);
        });

    }, []);

    const incidentCollectionRef = useMemo(() => {
        if (!db || !userId) return null;
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
        // Path: /artifacts/{appId}/users/{userId}/bandung_governance_incidents
        return collection(db, `artifacts/${appId}/users/${userId}/bandung_governance_incidents`);
    }, [db, userId]);

    return { db, auth, userId, isAuthReady, incidentCollectionRef };
};


// --- SIMULASI LOGIKA TATA KELOLA DIGITAL (BANDUNG RAYA) ---

// 1. Database Simulasi Peraturan Kota (Regulasi Lokal)
const REGULATORY_RULES = {
  // Aturan Kepatuhan Pengelolaan Sampah (Contoh Regulasi Bandung)
  WASTE_COMPLIANCE: {
    targetArea: 'Cihampelas',
    collectionSchedule: ['Monday', 'Wednesday', 'Friday'],
    maxVolume: 0.8, // Maksimum 80% volume kontainer sebelum dianggap over capacity
    fine: 500000,
  },
  // Aturan Kepatuhan Lalu Lintas Digital (Simulasi Sensor Lalu Lintas IoT)
  TRAFFIC_COMPLIANCE: {
    maxDensity: 0.75, // Kepadatan maksimum (75%) di jam sibuk
    sensorLocations: ['Dago', 'Pasteur', 'Gedebage'],
    alertLevel: 'Kritis',
  },
};

/**
 * Fungsi inti Umbra: Mengecek kepatuhan data simulasi terhadap regulasi lokal.
 * Mimensimulasikan Cipher Compliance Engine (CCE) untuk regulasi publik.
 * @param {object} data - Data sensor/IoT simulasi.
 * @returns {object} - Status kepatuhan dan insiden yang terdeteksi.
 */
const checkCompliance = (data) => {
  const incidents = [];
  let isOverallCompliant = true;

  // Cek Kepatuhan Sampah
  if (data.wasteVolume > REGULATORY_RULES.WASTE_COMPLIANCE.maxVolume) {
    isOverallCompliant = false;
    incidents.push({
      type: 'Sampah',
      area: REGULATORY_RULES.WASTE_COMPLIANCE.targetArea,
      description: `Volume sampah melebihi batas (${(data.wasteVolume * 100).toFixed(0)}%) di ${REGULATORY_RULES.WASTE_COMPLIANCE.targetArea}.`,
      status: 'Pelanggaran Berat',
      timestamp: serverTimestamp(), // Added for Firestore
    });
  }

  // Cek Kepatuhan Lalu Lintas
  if (data.trafficDensity > REGULATORY_RULES.TRAFFIC_COMPLIANCE.maxDensity) {
    isOverallCompliant = false;
    incidents.push({
      type: 'Lalu Lintas',
      area: 'Dago/Pasteur (Simulasi)',
      description: `Kepadatan lalu lintas kritis (${(data.trafficDensity * 100).toFixed(0)}%). Perlu intervensi sinyal otomatis.`,
      status: REGULATORY_RULES.TRAFFIC_COMPLIANCE.alertLevel,
      timestamp: serverTimestamp(), // Added for Firestore
    });
  }

  return { isOverallCompliant, incidents };
};

// --- REACT COMPONENT START ---

const App = () => {
  const { db, userId, isAuthReady, incidentCollectionRef } = useFirebase();

  const [governanceData, setGovernanceData] = useState({
    wasteVolume: 0.5,
    trafficDensity: 0.4,
    lastUpdate: new Date().toLocaleTimeString(),
  });
  const [incidents, setIncidents] = useState([]);
  const [systemStatus, setSystemStatus] = useState('Initializing...');
  const [isSimulationRunning, setIsSimulationRunning] = useState(false);


  // Fetch real-time incidents from Firestore
  useEffect(() => {
    if (!isAuthReady || !incidentCollectionRef) {
        setSystemStatus('Memuat Database...');
        return;
    }
    setSystemStatus('Memantau');
    setIsSimulationRunning(true);

    // Query: order by timestamp (descending) and limit to the 10 most recent incidents
    // NOTE: Firestore requires an index for orderBy. We sort client-side instead to avoid index requirement.
    const q = query(incidentCollectionRef);

    const unsubscribe = onSnapshot(q, (snapshot) => {
        const fetchedIncidents = snapshot.docs.map(doc => ({
            id: doc.id,
            ...doc.data(),
            // Ensure timestamp is a readable string for display
            timestamp: doc.data().timestamp?.toDate ? doc.data().timestamp.toDate().toLocaleTimeString() : 'N/A'
        }));
        
        // Sort client-side by timestamp in descending order
        fetchedIncidents.sort((a, b) => new Date(b.timestamp) - new Date(a.timestamp));
        setIncidents(fetchedIncidents);
    }, (error) => {
        console.error("Error fetching incidents:", error);
        setSystemStatus('DB Error');
    });

    return () => unsubscribe();
  }, [isAuthReady, incidentCollectionRef]);


  // Logika utama simulasi data dan cek kepatuhan
  const runDigitalScan = useCallback(async () => {
    if (!isSimulationRunning || !incidentCollectionRef) return;

    // Simulasi input data IoT (setiap tick)
    const newWasteVolume = Math.min(1.0, governanceData.wasteVolume + (Math.random() * 0.2 - 0.08));
    const newTrafficDensity = Math.min(1.0, governanceData.trafficDensity + (Math.random() * 0.3 - 0.15));

    const newData = {
      wasteVolume: newWasteVolume,
      trafficDensity: newTrafficDensity,
      lastUpdate: new Date().toLocaleTimeString(),
    };

    setGovernanceData(newData);

    // Lakukan Pengecekan Kepatuhan
    const { isOverallCompliant, incidents: newIncidents } = checkCompliance(newData);

    if (newIncidents.length > 0) {
        setSystemStatus('Insiden Terdeteksi');
        // Add new incidents to Firestore
        try {
            for (const incident of newIncidents) {
                // Remove the client-side timestamp field before adding to Firestore
                const { timestamp: clientTimestamp, ...incidentToSave } = incident; 
                await addDoc(incidentCollectionRef, incidentToSave);
            }
        } catch (error) {
            console.error("Failed to log incident to Firestore:", error);
        }
    } else {
        setSystemStatus('Komplian Penuh');
    }

  }, [isSimulationRunning, governanceData.wasteVolume, governanceData.trafficDensity, incidentCollectionRef]);

  // Hook untuk menjalankan pemindaian real-time
  useEffect(() => {
    let interval;
    if (isSimulationRunning && isAuthReady) {
        interval = setInterval(runDigitalScan, 4000); // Pemindaian setiap 4 detik
    }
    return () => clearInterval(interval);
  }, [isSimulationRunning, isAuthReady, runDigitalScan]);


  // Visualisasi Status Sistem
  const StatusDisplay = useMemo(() => {
    const isCompliant = incidents.length === 0 && systemStatus === 'Komplian Penuh';
    const color = incidents.length > 0 ? 'bg-red-600' : isCompliant ? 'bg-green-600' : 'bg-blue-600';
    const Icon = incidents.length > 0 ? XCircle : isCompliant ? ShieldCheck : Zap;

    return (
      <div className={`p-6 rounded-xl shadow-2xl transition duration-300 ${color} text-white`}>
        <div className="flex items-center justify-between">
          <h2 className="text-2xl font-bold">Status Sistem Tata Kelola</h2>
          <Icon className="w-8 h-8" />
        </div>
        <p className="text-4xl font-extrabold mt-2">{systemStatus}</p>
        <p className="text-sm mt-1 opacity-80">Terakhir diperbarui: {governanceData.lastUpdate}</p>
      </div>
    );
  }, [incidents.length, systemStatus, governanceData.lastUpdate]);

  // Kartu Data Real-time
  const DataCard = ({ title, value, unit, icon: Icon, isCritical }) => {
    const valueDisplay = (value * 100).toFixed(1);
    const color = isCritical ? 'text-red-400' : 'text-teal-400';
    return (
      <div className="bg-gray-800 p-5 rounded-xl border border-gray-700 shadow-md">
        <div className="flex items-center justify-between">
          <h3 className="text-lg font-semibold text-gray-300">{title}</h3>
          <Icon className="w-6 h-6 text-blue-400" />
        </div>
        <p className={`text-4xl font-extrabold mt-2 ${color}`}>
          {valueDisplay}
          <span className="text-xl font-medium ml-1 text-gray-400">{unit}</span>
        </p>
      </div>
    );
  };
  
  // Tampilan Utama
  if (!isAuthReady) {
    return (
        <div className="min-h-screen bg-gray-900 text-white flex items-center justify-center">
            <p className="flex items-center text-xl text-teal-400"><RefreshCw className="w-6 h-6 mr-2 animate-spin" /> Menginisialisasi Umbra DB...</p>
        </div>
    );
  }

  return (
    <div className="min-h-screen bg-gray-900 text-white font-sans p-4 sm:p-8">
      <script src="https://cdn.tailwindcss.com"></script>
      <style>
        {`
          @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;800&display=swap');
          body { font-family: 'Inter', sans-serif; }
        `}
      </style>

      <div className="max-w-4xl mx-auto">
        <header className="text-center mb-10">
          <h1 className="text-3xl sm:text-4xl font-black mb-2 text-transparent bg-clip-text bg-gradient-to-r from-teal-400 to-indigo-500">
            SISTEM TATA KELOLA DIGITAL BANDUNG RAYA
          </h1>
          <p className="text-gray-400">
            Kerangka Konseptual CCE dengan Data Persisten (Logged to Firestore for User: {userId})
          </p>
        </header>

        {/* Status Sistem */}
        <div className="mb-8">
            {StatusDisplay}
        </div>

        {/* Data Sensor Utama */}
        <h2 className="text-xl font-bold mb-4 text-gray-300 border-b border-gray-700 pb-2">
            Pemantauan Data (UmbraIoT Feed Simulasi)
        </h2>
        <div className="grid grid-cols-1 sm:grid-cols-2 gap-6 mb-8">
          <DataCard
            title="Volume Sampah (Cihampelas)"
            value={governanceData.wasteVolume}
            unit="%"
            icon={Truck}
            isCritical={governanceData.wasteVolume > 0.8}
          />
          <DataCard
            title="Kepadatan Lalu Lintas"
            value={governanceData.trafficDensity}
            unit="%"
            icon={BarChart2}
            isCritical={governanceData.trafficDensity > 0.75}
          />
        </div>

        {/* Insiden Pelanggaran */}
        <h2 className="text-xl font-bold mb-4 text-gray-300 border-b border-gray-700 pb-2 flex items-center">
            <Bell className="w-5 h-5 mr-2 text-yellow-400" />
            Log Insiden Kepatuhan ({incidents.length})
        </h2>
        <div className="space-y-4 max-h-96 overflow-y-auto pr-2">
          {incidents.length > 0 ? (
            incidents.map((incident) => (
              <div key={incident.id} className="bg-red-900/40 p-4 rounded-lg border border-red-700 flex items-start">
                <XCircle className="w-6 h-6 mr-3 mt-1 text-red-400 flex-shrink-0" />
                <div className='flex-grow'>
                  <p className="font-bold text-lg text-red-200">[{incident.type.toUpperCase()}] {incident.status}</p>
                  <p className="text-sm text-gray-300">{incident.description}</p>
                  <p className="text-xs text-gray-400 mt-1 italic">Logged: {incident.timestamp}</p>
                </div>
              </div>
            ))
          ) : (
            <div className="bg-green-900/30 p-4 rounded-lg text-center text-gray-400">
              <ShieldCheck className="w-5 h-5 inline mr-2" /> Tidak ada pelanggaran regulasi terdeteksi saat ini.
            </div>
          )}
        </div>
        
        {/* Footer Info */}
        <div className="mt-10 pt-6 border-t border-gray-700 text-center text-xs text-gray-500">
            <p>Simulasi Kerangka Kerja Ciptaan UmbraCode Forge (2025). Mengintegrasikan Logika Kepatuhan Digital dan Pemantauan IoT.</p>
        </div>
      </div>
    </div>
  );
};

export default App;
