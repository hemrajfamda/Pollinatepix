<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>PollinatePix Pro - AI Image Generator</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <script src="https://cdn.tailwindcss.com"></script>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  <style>
    .preview-img {
      max-height: 400px;
      width: 100%;
      object-fit: contain;
      border-radius: 1rem;
      box-shadow: 0 0 20px rgba(255, 255, 255, 0.1);
    }
    .footer-link:hover {
      text-decoration: underline;
    }
    .hamburger-menu {
      transform: translateX(-100%);
      transition: transform 0.3s ease-in-out;
    }
    .hamburger-menu.open {
      transform: translateX(0);
    }
    .coin-balance {
      background: linear-gradient(135deg, #f6d365 0%, #fda085 100%);
      -webkit-background-clip: text;
      background-clip: text;
      color: transparent;
    }
    .modal {
      transition: all 0.3s ease;
      opacity: 0;
      pointer-events: none;
    }
    .modal.active {
      opacity: 1;
      pointer-events: all;
    }
    .notification {
      animation: slideIn 0.3s forwards, fadeOut 0.5s 2.5s forwards;
    }
    @keyframes slideIn {
      from { transform: translateY(-50px); opacity: 0; }
      to { transform: translateY(0); opacity: 1; }
    }
    @keyframes fadeOut {
      from { opacity: 1; }
      to { opacity: 0; }
    }
    .subscription-btn {
      padding: 0.5rem 1rem;
      font-size: 0.875rem;
    }
  </style>
</head>
<body class="bg-gray-950 text-white min-h-screen transition-colors duration-300" id="body">
<!-- Sidebar Menu -->
<div class="hamburger-menu fixed top-0 left-0 w-64 h-full bg-gray-900 z-50 overflow-y-auto p-4 shadow-xl">
  <div class="flex justify-between items-center mb-8">
    <h2 class="text-xl font-bold text-purple-400">Premium Plans</h2>
    <button id="closeMenu" class="text-gray-400 hover:text-white">
      <i class="fas fa-times"></i>
    </button>
  </div>
  
  <div class="space-y-3">
    <div class="plan-card p-3 rounded-xl bg-gray-800 border border-purple-500">
      <h3 class="font-bold">₹19 Plan</h3>
      <p class="text-xs text-gray-300 mb-2">20 Credits/Day</p>
      <button class="w-full py-1.5 rounded-lg bg-green-600 hover:bg-green-700 text-xs font-medium subscription-btn pay-btn" data-plan="19" data-credits="20">
        Pay Now
      </button>
    </div>
    
    <div class="plan-card p-3 rounded-xl bg-gray-800 border border-yellow-500">
      <h3 class="font-bold">₹49 Plan</h3>
      <p class="text-xs text-gray-300 mb-2">50 Credits/Day</p>
      <button class="w-full py-1.5 rounded-lg bg-yellow-600 hover:bg-yellow-700 text-xs font-medium subscription-btn pay-btn" data-plan="49" data-credits="50">
        Pay Now
      </button>
    </div>
    
    <div class="plan-card p-3 rounded-xl bg-gray-800 border border-pink-500">
      <h3 class="font-bold">₹99 Plan</h3>
      <p class="text-xs text-gray-300 mb-2">100 Credits/Day</p>
      <button class="w-full py-1.5 rounded-lg bg-pink-600 hover:bg-pink-700 text-xs font-medium subscription-btn pay-btn" data-plan="99" data-credits="100">
        Pay Now
      </button>
    </div>
    
    <div class="plan-card p-3 rounded-xl bg-gray-800 border border-purple-300">
      <h3 class="font-bold">₹799 Plan</h3>
      <p class="text-xs text-gray-300 mb-2">Unlimited Credits/Year</p>
      <button class="w-full py-1.5 rounded-lg bg-purple-600 hover:bg-purple-700 text-xs font-medium subscription-btn pay-btn" data-plan="799" data-credits="unlimited">
        Pay Now
      </button>
    </div>
  </div>
  
  <div class="mt-6 pt-4 border-t border-gray-700">
    <div class="text-xs text-gray-400 mb-2">
      <i class="fas fa-lock mr-1"></i> Secure UPI Payments
    </div>
    <p class="text-xxs text-gray-500">Payment processed via UPI apps. No UPI ID displayed for security.</p>
  </div>
</div>

<script>
// Your UPI ID (Update here with your actual UPI ID)
const upiID = 'Q823382092@ybl';

// Function to redirect to UPI app based on selected plan
document.querySelectorAll('.pay-btn').forEach(button => {
  button.addEventListener('click', function() {
    const planAmount = this.getAttribute('data-plan');
    const credits = this.getAttribute('data-credits');

    // Create the UPI payment URL with your UPI ID
    const upiURL = `upi://pay?pa=${upiID}&pn=YourAppName&mc=123456&tid=1234567890&txnref=${Math.random().toString(36).substr(2, 9)}&am=${planAmount}&cu=INR&url=https://yourwebsite.com`;

    // Redirect to UPI app (Google Pay, Paytm, etc.)
    window.location.href = upiURL;

    // Optionally, log the action or track the transaction
    console.log(`Redirecting to UPI payment of ₹${planAmount} for ${credits} Credits.`);
  });
});
</script>
  <!-- Payment Processing Modal -->
  <div id="processingModal" class="modal fixed inset-0 bg-black bg-opacity-70 flex items-center justify-center z-50">
    <div class="bg-gray-800 rounded-xl p-6 max-w-md w-full mx-4 text-center">
      <div class="animate-spin rounded-full h-12 w-12 border-b-2 border-purple-500 mx-auto mb-4"></div>
      <h3 class="text-xl font-bold mb-2">Processing Payment</h3>
      <p class="text-sm text-gray-300">Redirecting to your UPI app...</p>
      <p class="text-xs text-gray-500 mt-4">If not redirected automatically, please check your UPI app</p>
    </div>
  </div>

  <!-- Payment Success Modal -->
  <div id="successModal" class="modal fixed inset-0 bg-black bg-opacity-70 flex items-center justify-center z-50">
    <div class="bg-gray-800 rounded-xl p-6 max-w-md w-full mx-4 text-center">
      <div class="w-16 h-16 bg-green-500 rounded-full flex items-center justify-center mx-auto mb-4">
        <i class="fas fa-check text-white text-2xl"></i>
      </div>
      <h3 class="text-xl font-bold mb-2">Payment Successful!</h3>
      <p class="text-sm text-gray-300 mb-4">Your account has been credited with <span id="creditedCoins" class="font-bold">0</span> credits.</p>
      <button id="closeSuccessModal" class="w-full py-2 rounded-lg bg-green-600 hover:bg-green-700 font-medium">
        Continue Generating
      </button>
    </div>
  </div>

  <!-- Credit Exhausted Modal -->
  <div id="creditModal" class="modal fixed inset-0 bg-black bg-opacity-70 flex items-center justify-center z-50">
    <div class="bg-gray-800 rounded-xl p-6 max-w-md w-full mx-4">
      <h3 class="text-xl font-bold mb-4">Credits Exhausted</h3>
      <p class="mb-4 text-sm">You've used all your free credits. Upgrade to a premium plan to continue generating images.</p>
      <div class="flex space-x-3">
        <button id="closeCreditModal" class="flex-1 py-2 rounded-lg bg-gray-600 hover:bg-gray-700 text-sm">Maybe Later</button>
        <button id="openPlansFromModal" class="flex-1 py-2 rounded-lg bg-purple-600 hover:bg-purple-700 text-sm">View Plans</button>
      </div>
    </div>
  </div>
</style>
<script>
  // Header Scroll Effect
  let lastScrollTop = 0;
  window.addEventListener("scroll", function () {
    const header = document.querySelector("header");
    const scrollTop = window.pageYOffset || document.documentElement.scrollTop;

    if (scrollTop > lastScrollTop) {
      // Scroll down
      header.classList.add("opacity-0", "-translate-y-full");
    } else {
      // Scroll up
      header.classList.remove("opacity-0", "-translate-y-full");
    }
    lastScrollTop = scrollTop <= 0 ? 0 : scrollTop;
  });
</script>
<header class="fixed top-0 left-0 right-0 bg-gray-900/80 backdrop-blur-md z-40 p-2 px-4 flex justify-between items-center shadow-lg border-b border-gray-800 transition-all duration-300">
  <!-- Sidebar -->
  <button id="menuButton" class="p-1.5 rounded-lg bg-gray-800 hover:bg-gray-700 transition shadow-sm">
    <i class="fas fa-bars text-white text-base"></i>
  </button>

  <!-- Coins -->
  <div class="flex items-center space-x-1 text-white text-xs">
    <span class="opacity-60">Credits:</span>
    <div class="coin-balance font-bold flex items-center bg-yellow-400/10 text-yellow-400 px-2 py-0.5 rounded-full shadow-inner">
      <i class="fas fa-coins mr-1"></i><span id="coinCount">5</span>
    </div>
  </div>

  <!-- Auth Buttons -->
  <div id="authButtons" class="flex items-center space-x-2">
    <button onclick="showLoginPopup()" class="px-3 py-1 rounded-full bg-gradient-to-r from-blue-500 to-indigo-600 hover:from-blue-600 hover:to-indigo-700 text-white text-xs font-medium shadow hover:scale-105 transition">
      Login
    </button>
    <button onclick="showSignupPopup()" class="px-3 py-1 rounded-full bg-gradient-to-r from-green-500 to-emerald-600 hover:from-green-600 hover:to-emerald-700 text-white text-xs font-medium shadow hover:scale-105 transition">
      Signup
    </button>
    <div id="userInfo" class="hidden text-white flex items-center space-x-2">
      <span id="userName" class="font-medium text-xs"></span>
      <button onclick="logoutUser()" class="px-3 py-1 rounded-full bg-red-600 hover:bg-red-700 text-xs text-white shadow transition">
        Logout
      </button>
    </div>
  </div>

  <!-- Dark Mode Button -->
  <button id="toggleDarkMode" class="p-2 rounded-full bg-gradient-to-br from-gray-800 to-gray-700 hover:from-gray-700 hover:to-gray-600 text-white text-sm shadow-lg hover:rotate-180 transition">
    <i class="fas fa-moon"></i>
  </button>
</header>
  <!-- Main Content -->
  <main class="flex flex-col items-center justify-start px-4 pt-20 pb-10 min-h-screen space-y-6">
    <div class="text-center">
      <h1 class="text-4xl font-bold mb-1">PollinatePix Pro</h1>
      <p class="text-xs text-purple-400">Powered by Pollinations AI | Made by <strong>Famda</strong></p>
    </div>

<!-- Container for Input & Voice Button -->
<div class="w-full max-w-xl relative">
  <!-- Input Box -->
  <input id="promptInput" type="text"
         placeholder="Describe your image (Hindi or English)..."
         class="w-full px-5 py-4 pr-14 rounded-2xl bg-[#1F1F1F] text-white text-base shadow-xl placeholder-gray-400 focus:outline-none focus:ring-2 focus:ring-purple-500 transition duration-300" />

  <!-- Microphone Button -->
  <button id="micBtn" onclick="toggleMic()"
          class="absolute top-1/2 right-3 transform -translate-y-1/2 w-10 h-10 flex items-center justify-center bg-white/10 backdrop-blur-md border border-white/20 rounded-full text-purple-400 hover:text-white hover:bg-purple-600 transition duration-300">
    <svg id="micIcon" xmlns="http://www.w3.org/2000/svg" fill="none"
         viewBox="0 0 24 24" stroke="currentColor" class="w-6 h-6">
      <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
            d="M12 1v2m0 16v4m6-4a6 6 0 01-12 0M19 10a7 7 0 10-14 0 7 7 0 0014 0z" />
    </svg>
  </button>
</div>

<script>
  let recognizing = false;
  let recognition;

  if ('SpeechRecognition' in window || 'webkitSpeechRecognition' in window) {
    recognition = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
    recognition.lang = 'hi-IN'; // or 'en-US' for English
    recognition.interimResults = false;
    recognition.maxAlternatives = 1;

    recognition.onresult = (event) => {
      const transcript = event.results[0][0].transcript;
      document.getElementById('promptInput').value = transcript;
    };

    recognition.onend = () => {
      recognizing = false;
      updateMicUI(false);
    };

    recognition.onerror = (event) => {
      console.error("Speech Recognition Error:", event.error);
      recognizing = false;
      updateMicUI(false);
    };
  }

  function toggleMic() {
    if (!recognition) return;

    if (recognizing) {
      recognition.stop();
      recognizing = false;
      updateMicUI(false);
    } else {
      recognition.start();
      recognizing = true;
      updateMicUI(true);
    }
  }

  function updateMicUI(isActive) {
    const micBtn = document.getElementById('micBtn');
    if (isActive) {
      micBtn.classList.add('animate-pulse', 'bg-purple-600', 'text-white');
    } else {
      micBtn.classList.remove('animate-pulse', 'bg-purple-600', 'text-white');
    }
  }
</script>
      <select id="styleSelect" class="w-full px-4 py-3 rounded-xl bg-gray-800 text-white focus:outline-none focus:ring-2 focus:ring-yellow-500 transition">
        <option value="">Select Style (Optional)</option>
        <option value="cartoon">Cartoon</option>
        <option value="ghibli">Ghibli</option>
        <option value="cyberpunk">Cyberpunk</option>
        <option value="fantasy">Fantasy</option>
        <option value="realistic">Realistic</option>
      </select>

      <select id="sizeSelect" class="w-full px-4 py-3 rounded-xl bg-gray-800 text-white focus:outline-none focus:ring-2 focus:ring-purple-500 transition">
        <option value="">Select Size (Optional)</option>
        <option value="1280x720">YouTube Thumbnail (1280x720)</option>
        <option value="1080x1080">Instagram Post (1080x1080)</option>
        <option value="1080x1920">Instagram Story (1080x1920)</option>
        <option value="1200x630">Facebook Post (1200x630)</option>
      </select>

      <button id="generateBtn" class="w-full py-3 rounded-xl bg-gradient-to-r from-purple-500 to-pink-500 hover:from-purple-600 hover:to-pink-600 font-semibold transition duration-300">
        Generate Image (1 Credit)
      </button>
    </div>

    <div id="loading" class="mt-6 hidden animate-pulse text-purple-400">
      <i class="fas fa-spinner fa-spin mr-2"></i> Generating image with Pollinations AI...
    </div>

    <!-- Image Grid -->
    <div id="imageGrid" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6 mt-10 w-full max-w-6xl px-2">
      <!-- Images appear here -->
    </div>
  </main>

  <!-- Footer -->
  <footer class="bg-gray-900 text-center py-6 px-4 mt-12">
    <p class="text-sm text-gray-400">
      &copy; 2025 PollinatePix Pro by Famda. All rights reserved.
    </p>
    <div class="flex justify-center gap-4 mt-2">
      <a href="#terms" class="text-xs text-purple-400 footer-link">Terms & Conditions</a>
      <a href="#privacy" class="text-xs text-purple-400 footer-link">Privacy Policy</a>
    </div>
  </footer>

  <!-- Terms and Conditions Section -->
  <section id="terms" class="max-w-4xl mx-auto px-4 py-10 text-sm text-gray-300">
    <h2 class="text-xl font-semibold text-purple-400 mb-4">Terms & Conditions</h2>
    <p>
      Welcome to PollinatePix. By using this website, you agree to comply with and be bound by the following terms. All content generated by AI is for personal use only. Any misuse, redistribution, or unauthorized commercial usage is prohibited.
    </p>
    <p class="mt-2">
      We reserve the right to modify or terminate services at any time. Users are responsible for the content they generate and must not use offensive, illegal, or harmful prompts.
    </p>
    <p class="mt-2">
      All payments are processed via secure UPI gateways. Refunds are not available for digital credits once purchased. Subscription plans auto-renew unless canceled.
    </p>
  </section>

  <!-- Privacy Policy Section -->
  <section id="privacy" class="max-w-4xl mx-auto px-4 py-10 text-sm text-gray-300">
    <h2 class="text-xl font-semibold text-purple-400 mb-4">Privacy Policy</h2>
    <p>
      PollinatePix values your privacy. We do not store any personal information or prompt history. No cookies or tracking tools are used for profiling.
    </p>
    <p class="mt-2">
      All image generations happen via API calls to Pollinations AI. Your input is not saved or shared. We ensure a safe and private experience for all users.
    </p>
    <p class="mt-2">
      Payment processing is handled securely by UPI providers. We do not store any payment information on our servers.
    </p>
  </section>

  <script>
    // DOM Elements
    const generateBtn = document.getElementById('generateBtn');
    const loading = document.getElementById('loading');
    const imageGrid = document.getElementById('imageGrid');
    const toggleDarkMode = document.getElementById('toggleDarkMode');
    const body = document.getElementById('body');
    const menuButton = document.getElementById('menuButton');
    const closeMenu = document.getElementById('closeMenu');
    const hamburgerMenu = document.querySelector('.hamburger-menu');
    const payButtons = document.querySelectorAll('.pay-btn');
    const processingModal = document.getElementById('processingModal');
    const successModal = document.getElementById('successModal');
    const closeSuccessModal = document.getElementById('closeSuccessModal');
    const creditedCoins = document.getElementById('creditedCoins');
    const creditModal = document.getElementById('creditModal');
    const closeCreditModal = document.getElementById('closeCreditModal');
    const openPlansFromModal = document.getElementById('openPlansFromModal');
    const coinCount = document.getElementById('coinCount');

    // State
    let isDark = true;
    let coins = 5; // Initial coins
    let selectedPlan = null;

    // Initialize
    updateCoinDisplay();
    checkURLForPaymentStatus();

    // Check URL for payment status (simulated UPI callback)
    function checkURLForPaymentStatus() {
      const urlParams = new URLSearchParams(window.location.search);
      const paymentStatus = urlParams.get('payment_status');
      
      if (paymentStatus === 'success') {
        // In a real app, you would verify the transaction server-side
        const plan = urlParams.get('plan');
        if (plan) {
          addCreditsForPlan(plan);
          showSuccessModal(plan);
        }
        
        // Clean URL
        window.history.replaceState({}, document.title, window.location.pathname);
      }
    }

    // Menu Toggle
    menuButton.addEventListener('click', () => {
      hamburgerMenu.classList.add('open');
    });

    closeMenu.addEventListener('click', () => {
      hamburgerMenu.classList.remove('open');
    });

    // Payment Buttons
    payButtons.forEach(button => {
      button.addEventListener('click', () => {
        selectedPlan = {
          amount: button.getAttribute('data-plan'),
          credits: button.getAttribute('data-credits')
        };
        initiateUPIPayment(selectedPlan);
      });
    });

    // UPI Payment Flow
    function initiateUPIPayment(plan) {
      processingModal.classList.add('active');
      hamburgerMenu.classList.remove('open');
      
      // Simulate UPI redirection (in real app, this would be a UPI intent URL)
      setTimeout(() => {
        processingModal.classList.remove('active');
        
        // In a real app, this would be the UPI callback URL
        // For demo, we'll simulate successful payment after 2 seconds
        simulateSuccessfulPayment(plan);
      }, 2000);
    }

    function simulateSuccessfulPayment(plan) {
      addCreditsForPlan(plan.credits);
      showSuccessModal(plan.credits);
    }

    function addCreditsForPlan(credits) {
      if (credits === 'unlimited') {
        coins = Infinity;
      } else {
        coins += parseInt(credits);
      }
      updateCoinDisplay();
    }

    function showSuccessModal(credits) {
      creditedCoins.textContent = credits === 'unlimited' ? 'Unlimited' : credits;
      successModal.classList.add('active');
    }

    // Modals
    closeSuccessModal.addEventListener('click', () => {
      successModal.classList.remove('active');
    });

    openPlansFromModal.addEventListener('click', () => {
      creditModal.classList.remove('active');
      hamburgerMenu.classList.add('open');
    });

    closeCreditModal.addEventListener('click', () => {
      creditModal.classList.remove('active');
    });

    // Dark Mode Toggle
    toggleDarkMode.addEventListener('click', () => {
      isDark = !isDark;
      body.classList.toggle('bg-gray-950');
      body.classList.toggle('text-white');
      body.classList.toggle('bg-white');
      body.classList.toggle('text-black');
      toggleDarkMode.innerHTML = isDark ? '<i class=
