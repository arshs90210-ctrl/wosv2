/* --- WorldOS: Sovereign Data Funds Platform - ROBUST PoU REDEVELOPMENT --- */

<script type="module">
    // Import only necessary Firestore functions for data simulation
    import { getFirestore, doc, setDoc, updateDoc, onSnapshot, collection, query, getDocs, arrayUnion, addDoc, serverTimestamp, orderBy } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";
    import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
    
    // Expose Firebase objects globally within the module scope
    const firebase = {
        initializeApp,
        getFirestore, doc, setDoc, updateDoc, onSnapshot, collection, query, getDocs,
        arrayUnion, addDoc, serverTimestamp, orderBy
    };
    
    // --- GLOBAL CONSTANTS ---
    const DB_NAME = 'WorldOS_SovereignNode_Robust';
    const DB_VERSION = 1;

    // BASE CONFIG FOR ROBUST PoU
    const PoU_CONFIG = {
        BASE_SCORE: 10, // Starting PoU score for any deposited asset
        MAX_SCORE: 100,
        CREDIBILITY_MULTIPLIERS: { // Factor 1: Credibility
            'Verified': 1.25, // e.g., signed PDFs, government data
            'High': 1.10, // e.g., direct API exports, trusted reports
            'Medium': 1.00, // e.g., standard downloaded reports, parsed data
            'Low': 0.85 // e.g., scraped content, unverified
        },
        GRANULARITY_ADDENDUM: { // Factor 2: Granularity (Data Set Complexity/Usefulness)
            'High': 15, // e.g., time-series data, transactional data
            'Medium': 10, // e.g., summaries, aggregated stats
            'Low': 5 // e.g., simple documents, unstructured text
        },
        QUERY_BOOST_RANGE: [15, 30] // PoU increase from a successful AI/RAG query
    };

    // FIXED: Dummy User ID and Type for immediate access
    let db;
    let userId = 'DUMMY_INDIVIDUAL_USER_001';
    let userType = 'individual';
    let userProfile = { totalYield: 0, funds: {}, userType: 'individual' };

    // Dummy Firebase Config/App ID (Needed for Firestore paths)
    const __app_id = 'worldos-sovereign-node-demo';
    const firebaseConfig = {
        apiKey: "AIzaSy_dummy-api-key-for-test-ONLY",
        authDomain: "dummy-project.firebaseapp.com",
        projectId: "dummy-project",
        storageBucket: "dummy-project.appspot.com",
        messagingSenderId: "123456789012",
        appId: "1:123456789012:web:a1b2c3d4e5f6g7h8"
    };

    // --- GLOBAL STATE ---
    let WorldState = {
        db: null, // IndexedDB instance
        dataFunds: [] // Firestore funds metadata
    };
    
    // --- 0. DATA PERSISTENCE (INDEXEDDB - LOCAL CONTENT) ---
    const LocalDB = {
        init: () => {
            return new Promise((resolve, reject) => {
                const request = indexedDB.open(DB_NAME, DB_VERSION);
                request.onupgradeneeded = (e) => {
                    WorldState.db = e.target.result;
                    WorldState.db.createObjectStore('files', { keyPath: 'id' });
                };
                request.onsuccess = (e) => { WorldState.db = e.target.result; resolve(); };
                request.onerror = (e) => { console.error("IndexedDB Error:", e.target.error); reject(e.target.error); };
            });
        },
        getStore: (storeName, mode) => WorldState.db.transaction(storeName, mode).objectStore(storeName),
        putFile: (file) => new Promise((resolve, reject) => {
            const request = LocalDB.getStore('files', 'readwrite').put(file);
            request.onsuccess = () => resolve(request.result);
            request.onerror = () => reject(request.error);
        }),
        getAllFiles: () => new Promise((resolve, reject) => {
            const request = LocalDB.getStore('files', 'readonly').getAll();
            request.onsuccess = () => resolve(request.result);
            request.onerror = () => reject(request.error);
        })
    };

    // --- CORE ROUTER (EXPOSED) ---
    const Router = window.Router = {
        nav: (viewId, element) => {
            document.querySelectorAll('.view-section').forEach(el => {
                el.classList.add('hidden');
                el.classList.remove('fade-in');
            });
            
            const targetView = document.getElementById('view-' + viewId);
            targetView.classList.remove('hidden');
            targetView.classList.add('fade-in');

            document.querySelectorAll('.nav-item').forEach(el => el.classList.remove('active'));
            if (element) element.classList.add('active');
            
            document.getElementById('breadcrumb').innerText = `SYSTEM // ${viewId.toUpperCase()}`;

            if (viewId === 'vault') UI.renderVault();
            if (viewId === 'ledger') UI.renderLedger();
            if (viewId === 'marketplace') DataFunds.render();
            if (viewId === 'intelligence') UI.renderIntelligenceView();
        }
    };

    // --- FIREBASE INITIALIZATION (Simplified to only initialize Firestore) ---
    const FirebaseInit = {
        setup: async () => {
            try {
                const app = firebase.initializeApp(firebaseConfig);
                db = firebase.getFirestore(app);
                
                FirebaseInit.loadUserProfile(userId);
                UI.enableApp();
                
            } catch (e) {
                console.error("Firebase setup failed:", e);
            }
        },
        loadUserProfile: (uid) => {
            const userDocRef = firebase.doc(db, "artifacts", __app_id, "users", uid, "profile", "data");
            
            firebase.onSnapshot(userDocRef, (docSnap) => {
                if (docSnap.exists()) {
                    userProfile = docSnap.data();
                    userType = userProfile.userType || 'individual';
                    // Allow company switch only if it's the right ID, otherwise revert
                    if (userProfile.userType === 'company' && uid !== 'DUMMY_COMPANY_USER_001') {
                        userId = 'DUMMY_COMPANY_USER_001';
                        // Rerun with correct ID to load the company profile
                        FirebaseInit.loadUserProfile(userId); 
                        return;
                    }
                    if (userProfile.userType === 'individual' && uid !== 'DUMMY_INDIVIDUAL_USER_001') {
                        userId = 'DUMMY_INDIVIDUAL_USER_001';
                        FirebaseInit.loadUserProfile(userId);
                        return;
                    }
                } else {
                    // Create a default profile if it doesn't exist
                    userProfile = { totalYield: 0, funds: {}, userType: userType };
                    firebase.setDoc(userDocRef, userProfile);
                }
                UI.updateHeader();
                DataFunds.listen();
                UI.renderLedger();
            }, (error) => console.error("Error loading user profile:", error));
        }
    };

    // --- CORE LOGIC BLOCKS ---
    
    const CryptoCore = {
        async sha256(message) {
            const msgBuffer = new TextEncoder().encode(message);
            const hashBuffer = await crypto.subtle.digest('SHA-256', msgBuffer);
            const hashArray = Array.from(new Uint8Array(hashBuffer));
            return hashArray.map(b => b.toString(16).padStart(2, '0')).join('');
        }
    };
    
    const PoU = {
        // Factor 1: Credibility (Based on file extension/source)
        determineCredibility: (filename) => {
            const ext = filename.split('.').pop().toLowerCase();
            if (['pdf', 'xlsx', 'db', 'sql'].includes(ext) || filename.includes('signed')) return 'Verified';
            if (['csv', 'json', 'xml'].includes(ext)) return 'High';
            if (['txt', 'html'].includes(ext)) return 'Medium';
            return 'Low';
        },
        // Factor 2: Granularity (Based on content size and assumed structure)
        determineGranularity: (content) => {
            const lines = content.split('\n').length;
            if (content.length > 5000 && lines > 50) return 'High';
            if (content.length > 1000 && lines > 10) return 'Medium';
            return 'Low';
        },
        // Factor 3: Market Demand (PoU Boost from Allocation - Implemented in DataFunds.toggleFundAllocation)
        
        // Initial PoU Calculation: BASE * CREDIBILITY + GRANULARITY
        calculateInitialPoU: (filename, content) => {
            const credibility = PoU.determineCredibility(filename);
            const granularity = PoU.determineGranularity(content);
            
            const multiplier = PoU_CONFIG.CREDIBILITY_MULTIPLIERS[credibility];
            const addendum = PoU_CONFIG.GRANULARITY_ADDENDUM[granularity];
            
            let score = PoU_CONFIG.BASE_SCORE * multiplier + addendum;
            return Math.min(PoU_CONFIG.MAX_SCORE, Math.round(score));
        },
        
        // PoU Boost from RAG Query (AI.send)
        applyQueryBoost: (currentScore) => {
            const [min, max] = PoU_CONFIG.QUERY_BOOST_RANGE;
            const boost = min + Math.floor(Math.random() * (max - min));
            const newScore = currentScore + boost;
            return Math.min(PoU_CONFIG.MAX_SCORE, newScore);
        }
    };

    const Vault = window.Vault = {
        ingest: async (input) => {
            if (userType !== 'individual') return alert('Please switch to Individual Data Sovereign mode to deposit files.');
            const file = input.files[0];
            if (!file) return;

            const text = await file.text();
            const fileId = crypto.randomUUID();

            const podHash = await CryptoCore.sha256(text + Date.now());
            // **ROBUST PoU 1: Use new categorization logic**
            const categories = Vault.categorize(file.name, text);
            const initialPouScore = PoU.calculateInitialPoU(file.name, text);
            
            const fileMetadata = {
                id: fileId,
                name: file.name,
                sizeKB: (file.size / 1024).toFixed(2),
                categories: categories, // Store as array for easy lookup
                marketCategory: categories.join(', '), // Store as comma-separated string for display
                credibility: PoU.determineCredibility(file.name),
                granularity: PoU.determineGranularity(text),
                pouScore: initialPouScore, // New initial score calculation
                isAllocated: false,
                podHash: podHash,
                ownerId: userId,
                timestamp: firebase.serverTimestamp()
            };

            await LocalDB.putFile({ id: fileId, content: text, ...fileMetadata });
            
            const metadataColRef = firebase.collection(db, "artifacts", __app_id, "users", userId, "fileMetadata");
            await firebase.addDoc(metadataColRef, fileMetadata);
            
            const ledgerColRef = firebase.collection(db, "artifacts", __app_id, "users", userId, "userLedger");
            await firebase.addDoc(ledgerColRef, {
                hash: podHash,
                action: `DEPOSIT: ${file.name} (${categories.join('/')})`,
                type: 'POD',
                details: `Initial PoU: ${initialPouScore}%`,
                timestamp: firebase.serverTimestamp()
            });
            
            alert(`SECURE DEPOSIT COMPLETE: Initial PoU Score: ${initialPouScore}%. Now go to the **Neural Core** and run a query to boost its utility score.`);
            input.value = '';
            UI.renderVault();
            UI.updateHeader();
        },
        // **ROBUST PoU 2: Expanded, Scalable Categorization**
        categorize: (filename, content) => {
            const lowFilename = filename.toLowerCase();
            const lowContent = content.toLowerCase();
            const categories = [];

            if (lowFilename.includes('bank') || lowFilename.includes('tax') || lowContent.includes('chase') || lowContent.includes('amex')) {
                categories.push('Financial/Tax');
            }
            if (lowFilename.includes('mobility') || lowFilename.includes('ev') || lowContent.includes('tesla') || lowContent.includes('uber') || lowContent.includes('route')) {
                categories.push('Urban/Mobility');
            }
            if (lowFilename.includes('shopping') || lowContent.includes('amazon') || lowContent.includes('target') || lowContent.includes('receipt')) {
                categories.push('Retail/E-commerce');
            }
            if (lowFilename.includes('health') || lowFilename.includes('fitness') || lowContent.includes('heart rate') || lowContent.includes('steps')) {
                categories.push('Health/Wellness');
            }
            if (lowFilename.includes('web') || lowFilename.includes('search') || lowContent.includes('google') || lowContent.includes('browser')) {
                categories.push('Digital/Web');
            }
            return categories.length > 0 ? categories : ['General/Other'];
        },
    };

    const DataFunds = window.DataFunds = {
        listen: () => {
            const fundsColRef = firebase.collection(db, "artifacts", __app_id, "public", "data", "dataFunds");
            const q = firebase.query(fundsColRef);
            firebase.onSnapshot(q, (snapshot) => {
                WorldState.dataFunds = snapshot.docs.map(doc => ({ id: doc.id, ...doc.data() }));
                // Initialize funds if they are missing (for demo purposes)
                if (WorldState.dataFunds.length === 0) {
                     DataFunds.seedFunds();
                } else {
                    DataFunds.render();
                }
            }, (error) => console.error("Error loading funds:", error));
        },
        seedFunds: async () => {
            const fundsColRef = firebase.collection(db, "artifacts", __app_id, "public", "data", "dataFunds");
            const initialFunds = [
                { id: 'FinancialTaxFund', name: 'Normalized Tax & Finance Fund', category: 'Financial/Tax', requiredPoU: 75, currentYield: 12.5, description: 'High-value fund for global CPA and wealth management analytics.', commission: 0.15 },
                { id: 'UrbanEVFund', name: 'Urban EV Adopters Fund', category: 'Urban/Mobility', requiredPoU: 50, currentYield: 4.8, description: 'Mobility patterns for urban planning and automotive industry analysis.', commission: 0.10 },
                { id: 'HealthInsightsFund', name: 'Verified Health & Wellness Fund', category: 'Health/Wellness', requiredPoU: 60, currentYield: 8.9, description: 'Longitudinal health data for medical research and preventative care.', commission: 0.20 },
                { id: 'DigitalTraceFund', name: 'Premium Digital Trace Fund', category: 'Digital/Web', requiredPoU: 40, currentYield: 3.2, description: 'Validated search/browser patterns for advertising and search optimization.', commission: 0.05 }
            ];
            await Promise.all(initialFunds.map(fund => firebase.setDoc(firebase.doc(fundsColRef, fund.id), fund)));
            // Re-listen will pick them up
        },
        render: () => {
            UI.renderDataFunds();
            UI.renderYieldPortfolio();
        },
        // **ROBUST PoU 3: Allocation Logic with PoU Requirement and WAC Calculation**
        toggleFundAllocation: async (fundId, isJoining) => {
            if (userType !== 'individual') return alert('Only individual users can allocate data to funds.');
            
            const files = await LocalDB.getAllFiles();
            const fund = WorldState.dataFunds.find(f => f.id === fundId);
            if (!fund) return alert('Fund not found.');

            // Find assets that match the fund's required category and PoU score
            const matchingAssets = files.filter(f => f.categories.includes(fund.category) && f.pouScore >= fund.requiredPoU);

            const userDocRef = firebase.doc(db, "artifacts", __app_id, "users", userId, "profile", "data");
            
            if (isJoining) {
                if (matchingAssets.length === 0) return alert(`Deposit files categorized as '${fund.category}' AND with a PoU score of at least ${fund.requiredPoU}% first to join this fund. Run a query in the Neural Core to boost PoU.`);
                if (userProfile.funds && userProfile.funds[fundId]) {
                    alert(`Already allocated to ${fundId}.`);
                    return;
                }
                
                // **ROBUST PoU 4: Weighted Asset Count (WAC) using PoU Score**
                const weightedAssetCount = matchingAssets.reduce((sum, f) => sum + (f.pouScore / 100), 0).toFixed(2);
                
                await firebase.updateDoc(userDocRef, {
                    [`funds.${fundId}`]: {
                        allocationTime: firebase.serverTimestamp(),
                        assetCount: parseFloat(weightedAssetCount),
                        commissionRate: fund.commission // Record the commission rate at time of allocation
                    }
                });
                alert(`Successfully allocated ${matchingAssets.length} eligible assets (WAC: ${weightedAssetCount}) to ${fund.name}!`);
            } else {
                if (!userProfile.funds || !userProfile.funds[fundId]) return alert('Not allocated to this fund.');
                
                let newFunds = { ...userProfile.funds };
                delete newFunds[fundId];
                await firebase.setDoc(userDocRef, { funds: newFunds }, { merge: true });
                alert(`Deallocated from ${fund.name}.`);
            }
            UI.renderYieldPortfolio();
            UI.renderVault();
            UI.updateHeader();
        },
        // **ROBUST PoU 5: Scalable Payout Logic**
        simulatePayout: async () => {
            if (userType !== 'company') return alert('Please switch to Enterprise Data Buyer mode to trigger a B2B usage/payout event.');

            const B2B_SDA_BUNDLE_PRICE = 50000;
            const allUsersSnapshot = await firebase.getDocs(firebase.collection(db, "artifacts", __app_id, "users"));
            let totalAssetCountByFund = WorldState.dataFunds.reduce((acc, fund) => ({ ...acc, [fund.id]: 0 }), {});
            let userAssetMap = {}; // { uid: { fundId: { wac: X, commission: Y } } }

            // 1. Calculate the total Weighted Asset Count for *each* fund across all users
            for (const docSnap of allUsersSnapshot.docs) {
                const profile = docSnap.data();
                if (profile.userType === 'individual' && profile.funds) {
                    userAssetMap[docSnap.id] = {};
                    for (const fundId in profile.funds) {
                        const fundData = profile.funds[fundId];
                        const fundMetadata = WorldState.dataFunds.find(f => f.id === fundId);
                        if (fundData.assetCount > 0 && fundMetadata) {
                            totalAssetCountByFund[fundId] += fundData.assetCount;
                            userAssetMap[docSnap.id][fundId] = { 
                                wac: fundData.assetCount, 
                                commission: fundMetadata.commission 
                            };
                        }
                    }
                }
            }
            
            // For a single, large B2B purchase, we'll assign the whole bundle price to the fund with the highest total WAC (Simulated)
            const activeFunds = Object.keys(totalAssetCountByFund).filter(id => totalAssetCountByFund[id] > 0);
            if (activeFunds.length === 0) return alert('No users currently contributing assets to any fund.');

            const targetFundId = activeFunds.reduce((a, b) => totalAssetCountByFund[a] > totalAssetCountByFund[b] ? a : b);
            const targetFundMetadata = WorldState.dataFunds.find(f => f.id === targetFundId);
            const totalWAC = totalAssetCountByFund[targetFundId];
            
            const commissionRate = targetFundMetadata.commission;
            const commission = B2B_SDA_BUNDLE_PRICE * commissionRate;
            const payoutPool = B2B_SDA_BUNDLE_PRICE - commission;

            // 2. Distribute payout proportionally to users contributing to the target fund
            const updatePromises = [];
            const yieldPayouts = {};

            for (const uid in userAssetMap) {
                if (userAssetMap[uid][targetFundId]) {
                    const userWAC = userAssetMap[uid][targetFundId].wac;
                    const share = userWAC / totalWAC;
                    const payout = payoutPool * share;
                    yieldPayouts[uid] = payout;
                    
                    const userRef = firebase.doc(db, "artifacts", __app_id, "users", uid, "profile", "data");
                    updatePromises.push(firebase.updateDoc(userRef, {
                        totalYield: firebase.arrayUnion({
                            amount: payout,
                            date: firebase.serverTimestamp(),
                            source: `${targetFundId} Sale`,
                            details: `WAC: ${userWAC.toFixed(2)} / Total WAC: ${totalWAC.toFixed(2)}`
                        })
                    }));
                }
            }

            // 3. Log the transaction for the Company User
            const companyLedgerRef = firebase.collection(db, "artifacts", __app_id, "users", userId, "userLedger");
            await firebase.addDoc(companyLedgerRef, {
                action: `B2B SDA Bundle Purchase: ${targetFundId}`,
                details: `Paid $${B2B_SDA_BUNDLE_PRICE.toFixed(2)}. Yield Pool: $${payoutPool.toFixed(2)}. Commission: $${commission.toFixed(2)} (${(commissionRate*100).toFixed(0)}%)`,
                timestamp: firebase.serverTimestamp(),
                type: 'COMPANY_EXPENSE',
                hash: await CryptoCore.sha256(JSON.stringify(yieldPayouts) + targetFundId)
            });

            await Promise.all(updatePromises);
            alert(`SUCCESS: Simulated B2B Purchase for **${targetFundId}** and distributed $${payoutPool.toFixed(2)} yield across ${Object.keys(yieldPayouts).length} users!`);
            UI.updateHeader();
            UI.renderYieldPortfolio();
            UI.renderLedger();
        }
    };

    const AI = window.AI = {
        send: async () => {
            if (userType !== 'individual') return alert('Only individual users can generate Proof of Utility blocks.');
            const input = document.getElementById('ai-input');
            const query = input.value.trim().toLowerCase();
            if(!query) return;

            AI.appendChat('user', input.value);
            input.value = '';

            AI.appendChat('ai', 'Thinking...', true);
            
            setTimeout(async () => {
                const files = await LocalDB.getAllFiles();
                // Find all assets where ANY of its categories are mentioned in the query
                const hitCategories = Vault.categorize('query_placeholder', query);
                const hits = files.filter(f => f.categories.some(cat => hitCategories.includes(cat)));

                let response = "";
                const lastMsg = document.querySelector('.loading-msg');
                
                if (hits.length > 0) {
                    const merkelRoot = await CryptoCore.sha256(query + hits.map(h => h.id).sort().join(''));
                    
                    let totalBoost = 0;
                    const affectedFundIds = [];
                    
                    // **ROBUST PoU 6: Apply PoU Boost to ALL relevant assets**
                    for (const hit of hits) {
                        const originalScore = hit.pouScore;
                        hit.pouScore = PoU.applyQueryBoost(originalScore);
                        totalBoost += (hit.pouScore - originalScore);
                        await LocalDB.putFile(hit); // Update local file in IndexedDB
                        
                        // Track which funds are affected by this boost
                        WorldState.dataFunds.forEach(fund => {
                            if (hit.categories.includes(fund.category) && !affectedFundIds.includes(fund.id)) {
                                affectedFundIds.push(fund.id);
                            }
                        });
                    }

                    const avgBoost = (totalBoost / hits.length).toFixed(1);

                    const ledgerColRef = firebase.collection(db, "artifacts", __app_id, "users", userId, "userLedger");
                    await firebase.addDoc(ledgerColRef, { 
                        action: `POU: Generated RAG Block for ${hitCategories.join('/')}`,
                        type: 'POU',
                        hash: merkelRoot,
                        fundIds: affectedFundIds,
                        details: `Boosted ${hits.length} assets by avg ${avgBoost} points.`,
                        timestamp: firebase.serverTimestamp()
                    });

                    response = `RAG Inference Complete. Analyzed **${hits.length}** relevant assets.\n\n`;
                    response += `**Proof of Utility (PoU) Block Generated:** This proves data normalization and verification. Each processed asset's PoU Score increased by an average of **${avgBoost} points**.\n\n`;
                    response += `> **Affected Funds:** ${affectedFundIds.length > 0 ? affectedFundIds.join(', ') : 'None'}. Check the Ledger for the Merkle Root hash and the Dashboard for your updated Live Average PoU Score.`;

                    // Re-render affected components
                    UI.renderLedger();
                    UI.renderVault();
                    UI.updateHeader();
                } else {
                    response = `I searched your local vault but found no assets matching the categories implied by that query: ${hitCategories.join(', ')}. Upload relevant files first (e.g., documents containing 'tax' or 'route' data).`;
                }

                if (lastMsg) lastMsg.remove();
                AI.appendChat('ai', response);

            }, 1200);
        },
        appendChat: (role, text, isLoading = false) => {
            // ... (rest of the AI.appendChat function remains the same)
            const container = document.getElementById('chat-history');
            const div = document.createElement('div');
            
            const baseClasses = `flex items-start gap-4 ${role === 'user' ? 'flex-row-reverse' : ''} ${isLoading ? 'loading-msg' : ''}`;
            // Apply the Gemini/ChatGPT clean bubble style
            const contentClasses = role === 'ai' 
                ? 'bg-panel border border-border text-primary' 
                : 'bg-accent text-accent-inv';
            
            const avatarContent = role === 'ai'
                ? '<i data-feather="cpu" class="w-4 h-4 text-indicator-ok"></i>'
                : '<i data-feather="user" class="w-4 h-4 text-surface"></i>'; // User icon uses surface for high contrast on teal

            // Use strong accent color for bolded text in AI response
            const formattedText = text.replace(/\*\*(.*?)\*\*/g, `<b style="color: var(--accent);">$1</b>`).replace(/\n/g, '<br>');

            div.className = baseClasses;
            div.innerHTML = `
                <div class="w-8 h-8 rounded ${role === 'ai' ? 'bg-panel border border-border' : 'bg-accent'} flex items-center justify-center shrink-0">
                    ${avatarContent}
                </div>
                <div class="${contentClasses} rounded-xl p-4 max-w-2xl text-sm leading-relaxed shadow-lg">
                    ${formattedText}
                </div>
            `;
            container.appendChild(div);
            container.scrollTop = container.scrollHeight;
            feather.replace();
        }
    };

    const UI = window.UI = {
        toggleUserType: () => {
            if (userType === 'individual') {
                userType = 'company';
                userId = 'DUMMY_COMPANY_USER_001';
            } else {
                userType = 'individual';
                userId = 'DUMMY_INDIVIDUAL_USER_001';
            }
            
            FirebaseInit.loadUserProfile(userId);
            Router.nav('dashboard', document.querySelector('.nav-item.active'));
            alert(`Switched to **${userType === 'company' ? 'Enterprise Data Buyer' : 'Individual Data Sovereign'}** mode. ID: ${userId}`);
        },
        enableApp: () => {
            document.querySelectorAll('.view-section').forEach(el => el.style.pointerEvents = 'auto');
            document.getElementById('user-display').innerText = `User ID: ${userId} | Type: ${userType}`;
        },
        disableApp: () => {
            document.querySelectorAll('.view-section').forEach(el => el.style.pointerEvents = 'none');
        },
        updateHeader: async () => {
            // Calculate Total Yield
            const totalYield = (userProfile.totalYield || []).reduce((sum, item) => sum + item.amount, 0);
            document.getElementById('dash-yield').innerText = `\$${totalYield.toFixed(2)}`;
            document.getElementById('dash-funds-count').innerText = Object.keys(userProfile.funds || {}).length;
            document.getElementById('current-user-type').innerText = userType === 'company' ? 'Enterprise' : 'Individual';

            // **ROBUST PoU 7: Live Average PoU Calculation**
            const files = await LocalDB.getAllFiles();
            const totalScore = files.reduce((sum, file) => sum + file.pouScore, 0);
            const avgPoU = files.length > 0 ? (totalScore / files.length).toFixed(0) : 0;
            const avgColor = avgPoU >= 75 ? 'text-indicator-ok' : avgPoU >= 50 ? 'text-indicator-warn' : 'text-accent'; // Dynamic Color
            document.getElementById('dash-pou-score').innerHTML = `<span class="${avgColor} font-code">${avgPoU}%</span>`;
            
            // Update storage usage
            const totalSizeKB = files.reduce((sum, file) => sum + parseFloat(file.sizeKB), 0);
            const displaySize = totalSizeKB > 1024 ? `${(totalSizeKB / 1024).toFixed(2)} MB` : `${totalSizeKB.toFixed(2)} KB`;
            document.getElementById('storage-usage').innerText = displaySize;
        },
        renderVault: async () => {
            const vaultHeader = document.getElementById('vault-header-text');
            const fileUpload = document.getElementById('file-upload-label');
            if (userType !== 'individual') {
                vaultHeader.innerText = 'Data Vault (Read-Only in Enterprise Mode)';
                fileUpload.classList.add('opacity-50', 'pointer-events-none');
            } else {
                vaultHeader.innerText = 'Data Vault 🛡️';
                fileUpload.classList.remove('opacity-50', 'pointer-events-none');
            }
            
            const tbody = document.getElementById('vault-list');
            tbody.innerHTML = ''; 

            const files = await LocalDB.getAllFiles();

            if (files.length === 0) {
                tbody.innerHTML = '<tr><td colspan="5" class="px-6 py-12 text-center text-secondary">Vault is empty. Deposit files to initialize local encryption.</td></tr>';
                return;
            }

            files.forEach(file => {
                const allocatedFundCategories = WorldState.dataFunds
                    .filter(fund => userProfile.funds && userProfile.funds[fund.id])
                    .map(fund => fund.category);
                    
                const isAllocated = file.categories.some(cat => allocatedFundCategories.includes(cat));
                
                const pouColor = file.pouScore >= 75 ? 'text-indicator-ok' : file.pouScore >= 50 ? 'text-indicator-warn' : 'text-accent';

                const tr = document.createElement('tr');
                tr.className = 'hover:bg-panel/50 transition-colors';
                tr.innerHTML = `
                    <td class="px-6 py-4 text-primary font-medium">${file.name}</td>
                    <td class="px-6 py-4 text-secondary font-code text-xs">${file.marketCategory}</td>
                    <td class="px-6 py-4 text-secondary font-code text-xs">${file.credibility}/${file.granularity}</td>
                    <td class="px-6 py-4 ${pouColor} font-code text-xs">${file.pouScore}%</td>
                    <td class="px-6 py-4">
                        <span class="${isAllocated ? 'bg-accent text-surface' : 'bg-secondary text-surface'} px-2 py-1 rounded text-[10px] font-semibold">
                            ${isAllocated ? 'ALLOCATED' : 'FREE'}
                        </span>
                    </td>
                `;
                tbody.prepend(tr);
            });
            feather.replace();
        },
        renderLedger: () => {
            // ... (Ledger rendering remains the same, but uses new details/types)
            if (!userId) return;
            const container = document.getElementById('ledger-list');
            container.innerHTML = '<div class="text-center text-secondary p-5">Loading Ledger...</div>';
            
            const ledgerColRef = firebase.collection(db, "artifacts", __app_id, "users", userId, "userLedger");
            const q = firebase.query(ledgerColRef, firebase.orderBy("timestamp", "desc"));

            firebase.onSnapshot(q, (snapshot) => {
                container.innerHTML = '';
                if (snapshot.empty) {
                    container.innerHTML = '<div class="text-center text-secondary p-5">Ledger is empty.</div>';
                    return;
                }
                snapshot.docs.forEach(docSnap => {
                    const block = docSnap.data();
                    const div = document.createElement('div');
                    const isYield = block.type === 'YIELD' || block.action.includes('Sale');
                    const isDeposit = block.type === 'POD' || block.action.startsWith('DEPOSIT');
                    const isPoU = block.type === 'POU';
                    const isCompanyExpense = block.type === 'COMPANY_EXPENSE';
                    
                    div.className = "ledger-entry p-4 rounded font-code text-xs space-y-2 fade-in";
                    div.innerHTML = `
                        <div class="flex justify-between text-secondary">
                            <span class="uppercase font-extrabold ${isPoU ? 'text-indicator-warn' : isDeposit ? 'text-accent' : isYield || isCompanyExpense ? 'text-indicator-ok' : 'text-primary'}">${block.type || 'TX'} ${isPoU && block.fundIds ? `(${block.fundIds.join(', ')})` : ''}</span>
                            <span class="text-[10px]">${block.timestamp ? new Date(block.timestamp.toDate()).toLocaleString() : 'Pending...'}</span>
                        </div>
                        <div class="text-primary font-semibold truncate">
                            ${block.action} ${block.details ? `— <span class="${isYield ? 'text-indicator-ok' : ''}">${block.details}</span>` : ''}
                        </div>
                        <span class="text-secondary text-[10px] block truncate opacity-70">HASH: ${block.hash ? block.hash.substring(0, 32) + '...' : 'N/A'}</span>
                    `;
                    container.appendChild(div);
                });
                UI.renderChart(snapshot.docs.map(doc => doc.data()));
            });
        },
        renderDataFunds: () => {
            const container = document.getElementById('data-funds-list');
            container.innerHTML = '';
            
            WorldState.dataFunds.forEach(fund => {
                const isAllocated = userProfile.funds && userProfile.funds[fund.id];
                const buttonText = userType === 'individual' ? (isAllocated ? 'DE-ALLOCATE' : 'ALLOCATE ASSETS') : 'ACCESS DATA STREAM';
                const buttonColor = userType === 'individual' 
                    ? (isAllocated ? 'bg-red-700 hover:bg-red-800' : 'bg-accent hover:bg-cyan-600')
                    : 'bg-green-700 hover:bg-green-800';
                
                const card = document.createElement('div');
                card.className = "bg-panel border border-border p-5 rounded-lg flex flex-col justify-between shadow-xl fade-in"; 
                card.innerHTML = `
                    <div>
                        <h4 class="text-xl font-extrabold mb-1 text-primary">${fund.name}</h4>
                        <p class="text-xs text-secondary mb-3 font-code uppercase">Category: <span class="text-accent">${fund.category}</span></p>
                        <p class="text-5xl font-code text-indicator-warn my-3 font-extrabold">${fund.currentYield}% <span class="text-base text-secondary font-medium">Yield Target</span></p>
                        <p class="text-sm text-secondary leading-relaxed mb-3">${fund.description}</p>
                        <div class="flex justify-between items-center mt-3 pt-3 border-t border-border">
                            <span class="text-xs text-secondary font-code">Min PoU: ${fund.requiredPoU}%</span>
                            <span class="text-xs text-secondary font-code text-right">WorldOS Comm.: ${(fund.commission * 100).toFixed(0)}%</span>
                        </div>
                    </div>
                    <div class="mt-4 flex justify-end">
                        <button onclick="${userType === 'individual' ? `DataFunds.toggleFundAllocation('${fund.id}', ${!isAllocated})` : `alert('Enterprise users only view access information here.')`}"
                            class="text-sm text-accent-inv px-4 py-2 rounded font-semibold transition-colors ${buttonColor} ${userType !== 'individual' && 'opacity-75 cursor-not-allowed'}">
                            ${buttonText}
                        </button>
                    </div>
                `;
                container.appendChild(card);
            });
        },
        renderYieldPortfolio: () => {
            // ... (Yield portfolio rendering remains the same, but uses WAC/commission)
            const tbody = document.getElementById('yield-portfolio-list');
            tbody.innerHTML = '';
            
            const activeFunds = userProfile.funds || {};
            const fundIds = Object.keys(activeFunds);
            
            if (fundIds.length === 0) {
                tbody.innerHTML = '<tr><td colspan="5" class="px-6 py-4 text-center text-secondary">You are not currently allocated to any fund.</td></tr>';
                
                const lifetimeYield = (userProfile.totalYield || []).reduce((sum, item) => sum + item.amount, 0);
                document.getElementById('dash-yield').innerText = `\$${(lifetimeYield).toFixed(2)}`;
                document.getElementById('dash-funds-count').innerText = fundIds.length;
                return;
            }
            
            fundIds.forEach(fundId => {
                const fund = WorldState.dataFunds.find(f => f.id === fundId);
                const userData = activeFunds[fundId];
                if (!fund) return;
                
                const lastPayout = (Math.random() * 50 + 10).toFixed(2); // Simulated Payout
                const commissionRate = userData.commissionRate || fund.commission; // Use saved rate or live rate
                const yieldShare = userData.assetCount * 100 / (Math.random() * 100 + userData.assetCount * 1.5) ; // Simulated yield share
                
                const tr = document.createElement('tr');
                tr.className = 'hover:bg-panel/50 transition-colors';
                tr.innerHTML = `
                    <td class="px-6 py-4 text-primary font-semibold">${fund.name}</td>
                    <td class="px-6 py-4 text-secondary font-code">${userData.assetCount.toFixed(2)} WAC</td>
                    <td class="px-6 py-4 text-secondary font-code">${(yieldShare).toFixed(2)}%</td>
                    <td class="px-6 py-4 text-secondary font-code">${(commissionRate * 100).toFixed(0)}%</td>
                    <td class="px-6 py-4">
                        <button onclick="DataFunds.toggleFundAllocation('${fundId}', false)" class="bg-red-700 text-accent-inv px-3 py-1 rounded text-xs font-semibold hover:bg-red-800 transition-colors" ${userType !== 'individual' ? 'disabled' : ''}>
                            De-Allocate
                        </button>
                    </td>
                `;
                tbody.appendChild(tr);
            });
            // Total yield update is now exclusively done in updateHeader, using totalYield from Firestore
            UI.updateHeader();
        },
        renderIntelligenceView: () => {
            // ... (Intelligence view rendering remains the same)
            const chatContainer = document.getElementById('intelligence-chat-container');
            const chatHistory = document.getElementById('chat-history');
            const inputContainer = document.getElementById('intelligence-input-container');

            if (userType === 'company') {
                chatContainer.classList.add('hidden');
                chatHistory.classList.add('hidden'); // Hide chat history when enterprise
                inputContainer.innerHTML = `
                    <div class="p-8 text-primary h-full">
                        <h2 class="text-3xl font-extrabold mb-4 text-accent">Enterprise Query Execution Pipeline 🚀</h2>
                        <p class="text-secondary mb-8 text-lg">As an Enterprise Data Buyer, this view shows the **currently executing RAG Queries** used to generate the next batch of tradable SDA bundles.</p>
                        <div class="bg-surface-dark border border-border p-6 rounded-lg space-y-4 shadow-xl">
                            <div class="font-code text-sm text-indicator-ok"><i data-feather="check-circle" class="w-4 h-4 inline-block mr-2"></i> Query 1: Normalized Q1 2024 Financial Spend (95% Complete)</div>
                            <div class="font-code text-sm text-indicator-warn"><i data-feather="loader" class="w-4 h-4 inline-block mr-2 animate-spin"></i> Query 2: Urban Mobility Trends Q3 2024 (23% Complete)</div>
                            <div class="font-code text-sm text-secondary"><i data-feather="clock" class="w-4 h-4 inline-block mr-2"></i> Pending: Retail Purchase Intent (Q4) Aggregation</div>
                        </div>
                        <p class="mt-4 text-xs text-secondary">Purchase the finished bundles in the **Data Funds & Yield** view (Simulate B2B Payout button).</p>
                    </div>
                `;
            } else {
                chatContainer.classList.remove('hidden');
                chatHistory.classList.remove('hidden'); // Show chat history for individual
                // Re-create the standard input field (handled dynamically in onload)
                inputContainer.innerHTML = `
                    <div class="max-w-4xl mx-auto relative">
                        <input type="text" id="ai-input" placeholder="Query your local data (e.g., 'Q3 tax data normalization')..." class="ai-input-style w-full rounded-lg py-3 pl-4 pr-12 text-sm font-medium focus:ring-1 focus:ring-accent">
                        <button onclick="AI.send()" class="absolute right-2 top-2 p-1.5 hover:bg-panel rounded text-secondary hover:text-accent transition-colors">
                            <i data-feather="arrow-up" class="w-4 h-4"></i>
                        </button>
                    </div>
                `;
                document.getElementById('ai-input').addEventListener('keypress', function (e) {
                    if (e.key === 'Enter') AI.send();
                });
                
                // Force chat history scroll down on view switch
                const container = document.getElementById('chat-history');
                container.scrollTop = container.scrollHeight;
            }
            feather.replace();
        },
        renderChart: (ledgerData) => {
            // ... (Chart rendering logic remains the same)
            const ctx = document.getElementById('vaultChart').getContext('2d');
            
            const pouDates = ledgerData
                .filter(b => b.type === 'POU')
                .map(b => (b.timestamp ? new Date(b.timestamp.toDate()).toLocaleDateString() : 'Pending'));

            const counts = pouDates.reduce((acc, date) => { acc[date] = (acc[date] || 0) + 1; return acc; }, {});

            let cumulative = 0;
            const labels = Object.keys(counts).sort();
            const data = labels.map(date => cumulative += counts[date]);

            if (window.vaultChartInstance) window.vaultChartInstance.destroy();

            window.vaultChartInstance = new Chart(ctx, {
                type: 'line',
                data: {
                    labels: labels,
                    datasets: [{
                        label: 'Cumulative PoU Blocks',
                        data: data,
                        borderColor: 'var(--accent)', 
                        backgroundColor: 'rgba(6, 182, 212, 0.2)', 
                        tension: 0.3,
                        fill: true,
                        pointRadius: 4, 
                        pointHoverRadius: 6
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: { legend: { display: false }, tooltip: { 
                        mode: 'index', 
                        intersect: false, 
                        titleFont: { family: 'JetBrains Mono', weight: 'bold' },
                        bodyFont: { family: 'JetBrains Mono' },
                        backgroundColor: 'var(--panel)', 
                        borderColor: 'var(--border)',
                        borderWidth: 1,
                        titleColor: 'var(--accent)',
                        bodyColor: 'var(--primary)'
                    } },
                    scales: {
                        y: { 
                            beginAtZero: true, 
                            grid: { color: 'rgba(30, 41, 59, 0.5)' }, 
                            ticks: { color: 'var(--text-sec)', font: { family: 'JetBrains Mono' } } 
                        },
                        x: { 
                            grid: { display: false }, 
                            ticks: { color: 'var(--text-sec)', font: { family: 'JetBrains Mono' } } 
                        }
                    }
                }
            });
        }
    };
    
    // --- INITIALIZATION BLOCK (Executes after module load) ---
    window.onload = async () => {
        feather.replace(); 
        await LocalDB.init();
        await FirebaseInit.setup();
        Router.nav('dashboard', document.querySelector('.nav-item.active'));

        // Re-attach event listener for the dynamic input in Individual mode
        const intelligenceView = document.getElementById('view-intelligence');
        new MutationObserver((mutationsList, observer) => {
            const input = document.getElementById('ai-input');
            if (input) {
                input.addEventListener('keypress', function(e) {
                    if (e.key === 'Enter') AI.send();
                }, { once: true });
            }
        }).observe(intelligenceView, { childList: true, subtree: true });
    };
</script>
