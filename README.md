// ========================================
// BIGBET - Professional Sports Betting Website
// JavaScript Functionality
// ========================================

// Sample Predictions Data
const predictionsData = [
    {
        id: 1,
        match: "Manchester United vs Liverpool",
        time: "20:00 UTC",
        prediction: "Over 2.5",
        confidence: 87,
        odds: 1.85
    },
    {
        id: 2,
        match: "Real Madrid vs Barcelona",
        time: "21:00 UTC",
        prediction: "Barcelona +1",
        confidence: 92,
        odds: 2.10
    },
    {
        id: 3,
        match: "Bayern Munich vs Dortmund",
        time: "19:30 UTC",
        prediction: "Bayern Win",
        confidence: 78,
        odds: 1.55
    },
    {
        id: 4,
        match: "PSG vs Marseille",
        time: "20:45 UTC",
        prediction: "PSG -1",
        confidence: 95,
        odds: 1.72
    },
    {
        id: 5,
        match: "Chelsea vs Arsenal",
        time: "15:00 UTC",
        prediction: "Draw",
        confidence: 65,
        odds: 3.40
    },
    {
        id: 6,
        match: "Inter Milan vs AC Milan",
        time: "18:00 UTC",
        prediction: "Inter Win",
        confidence: 88,
        odds: 1.95
    }
];

// Sample Results Data
const resultsData = [
    {
        match: "Liverpool vs Tottenham",
        prediction: "Over 2.5",
        result: "3-1",
        odds: 1.85,
        status: "won"
    },
    {
        match: "Arsenal vs Brighton",
        prediction: "Arsenal -1",
        result: "2-0",
        odds: 1.55,
        status: "won"
    },
    {
        match: "Manchester City vs Newcastle",
        prediction: "City -2",
        result: "2-1",
        odds: 2.15,
        status: "lost"
    },
    {
        match: "Chelsea vs Crystal Palace",
        prediction: "Chelsea Win",
        result: "1-1",
        odds: 1.45,
        status: "lost"
    },
    {
        match: "Aston Villa vs West Ham",
        prediction: "Over 2.5",
        result: "3-2",
        odds: 1.92,
        status: "won"
    },
    {
        match: "Everton vs Fulham",
        prediction: "Under 2.5",
        result: "2-2",
        odds: 1.68,
        status: "lost"
    }
];

// ========================================
// MOBILE MENU FUNCTIONALITY
// ========================================

function initializeMobileMenu() {
    const hamburger = document.getElementById('hamburger');
    const navMenu = document.getElementById('navMenu');

    if (hamburger) {
        hamburger.addEventListener('click', () => {
            navMenu.classList.toggle('active');
            hamburger.classList.toggle('active');
        });

        // Close menu when a link is clicked
        const navLinks = navMenu.querySelectorAll('a');
        navLinks.forEach(link => {
            link.addEventListener('click', () => {
                navMenu.classList.remove('active');
                hamburger.classList.remove('active');
            });
        });
    }
}

// ========================================
// LOAD PREDICTIONS
// ========================================

function loadPredictions() {
    const predictionsGrid = document.getElementById('predictionsGrid');
    
    if (!predictionsGrid) return;

    predictionsGrid.innerHTML = '';

    predictionsData.forEach(prediction => {
        const card = document.createElement('div');
        card.className = 'prediction-card';
        card.innerHTML = `
            <div class="match-time">
                <i class="fas fa-clock"></i> ${prediction.time}
            </div>
            <div class="match-name">${prediction.match}</div>
            <div class="prediction-details">
                <div class="prediction-text">${prediction.prediction}</div>
                <div style="color: var(--text-secondary);">
                    Odds: <strong style="color: var(--primary-gold);">${prediction.odds}</strong>
                </div>
            </div>
            <div class="confidence">
                <span class="confidence-value">${prediction.confidence}%</span>
                <span style="color: var(--text-secondary);">Confidence</span>
            </div>
            <div class="confidence-bar">
                <div class="confidence-fill" style="width: ${prediction.confidence}%;"></div>
            </div>
        `;
        predictionsGrid.appendChild(card);
    });
}

// ========================================
// LOAD RESULTS TABLE
// ========================================

function loadResults() {
    const resultsTableBody = document.getElementById('resultsTableBody');
    
    if (!resultsTableBody) return;

    resultsTableBody.innerHTML = '';

    resultsData.forEach(result => {
        const row = document.createElement('tr');
        const statusClass = result.status === 'won' ? 'status-won' : 'status-lost';
        const statusText = result.status === 'won' ? '✓ Won' : '✗ Lost';
        const statusIcon = result.status === 'won' ? 'fa-check-circle' : 'fa-times-circle';

        row.innerHTML = `
            <td>${result.match}</td>
            <td>${result.prediction}</td>
            <td><strong>${result.result}</strong></td>
            <td><strong>${result.odds}</strong></td>
            <td class="${statusClass}">
                <i class="fas ${statusIcon}"></i> ${statusText}
            </td>
        `;
        resultsTableBody.appendChild(row);
    });
}

// ========================================
// SCROLL ANIMATIONS
// ========================================

function initializeScrollAnimations() {
    const observerOptions = {
        threshold: 0.1,
        rootMargin: '0px 0px -100px 0px'
    };

    const observer = new IntersectionObserver((entries) => {
        entries.forEach(entry => {
            if (entry.isIntersecting) {
                entry.target.style.opacity = '1';
                entry.target.style.animation = 'fadeInUp 0.6s ease-out forwards';
            }
        });
    }, observerOptions);

    // Observe all prediction cards, stat cards, and contact cards
    document.querySelectorAll('.prediction-card, .stat-card, .contact-card').forEach(el => {
        el.style.opacity = '0';
        observer.observe(el);
    });
}

// ========================================
// COUNTER ANIMATION FOR STATS
// ========================================

function animateCounter(element, target, duration = 2000) {
    let current = 0;
    const increment = target / (duration / 16); // 60fps
    const startTime = Date.now();

    const counter = setInterval(() => {
        current += increment;
        if (current >= target) {
            current = target;
            clearInterval(counter);
        }
        element.textContent = Math.floor(current).toLocaleString();
    }, 16);
}

function initializeStatCounters() {
    const statCards = document.querySelectorAll('.stat-card');
    
    const observerOptions = {
        threshold: 0.5
    };

    const observer = new IntersectionObserver((entries) => {
        entries.forEach(entry => {
            if (entry.isIntersecting && !entry.target.dataset.animated) {
                const statValue = entry.target.querySelector('.stat-value');
                const target = parseInt(statValue.textContent.replace(/\D/g, ''));
                
                animateCounter(statValue, target);
                entry.target.dataset.animated = 'true';
            }
        });
    }, observerOptions);

    statCards.forEach(card => observer.observe(card));
}

// ========================================
// SMOOTH SCROLLING FOR ANCHOR LINKS
// ========================================

function initializeSmoothScroll() {
    document.querySelectorAll('a[href^="#"]').forEach(anchor => {
        anchor.addEventListener('click', function (e) {
            const href = this.getAttribute('href');
            if (href !== '#' && document.querySelector(href)) {
                e.preventDefault();
                document.querySelector(href).scrollIntoView({
                    behavior: 'smooth',
                    block: 'start'
                });
            }
        });
    });
}

// ========================================
// ACTIVE NAV LINK HIGHLIGHTING
// ========================================

function updateActiveNavLink() {
    const sections = document.querySelectorAll('section[id]');
    const navLinks = document.querySelectorAll('.nav-menu a');

    window.addEventListener('scroll', () => {
        let current = '';

        sections.forEach(section => {
            const sectionTop = section.offsetTop;
            const sectionHeight = section.clientHeight;
            if (scrollY >= sectionTop - 200) {
                current = section.getAttribute('id');
            }
        });

        navLinks.forEach(link => {
            link.classList.remove('active');
            if (link.getAttribute('href').slice(1) === current) {
                link.classList.add('active');
            }
        });
    });
}

// ========================================
// FORM INTERACTIONS
// ========================================

function initializeButtons() {
    // Subscribe buttons
    const subscribeButtons = document.querySelectorAll('.vip-card .btn');
    subscribeButtons.forEach(button => {
        button.addEventListener('click', function(e) {
            if (!this.href) {
                e.preventDefault();
                showNotification('Redirecting to payment...');
                setTimeout(() => {
                    // Replace with actual payment gateway URL
                    // window.location.href = 'https://payment.example.com';
                }, 1500);
            }
        });
    });

    // Contact buttons
    const contactButtons = document.querySelectorAll('.contact-card .btn');
    contactButtons.forEach(button => {
        button.addEventListener('mouseenter', function() {
            this.style.transform = 'scale(1.05)';
        });
        button.addEventListener('mouseleave', function() {
            this.style.transform = 'scale(1)';
        });
    });
}

// ========================================
// NOTIFICATION SYSTEM
// ========================================

function showNotification(message, type = 'info', duration = 3000) {
    const notification = document.createElement('div');
    notification.style.cssText = `
        position: fixed;
        top: 20px;
        right: 20px;
        background-color: var(--card-bg);
        color: var(--text-light);
        padding: 1rem 2rem;
        border-radius: 5px;
        border-left: 4px solid ${type === 'success' ? 'var(--success)' : 'var(--primary-gold)'};
        box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
        z-index: 10000;
        animation: slideInRight 0.3s ease-out;
    `;
    notification.textContent = message;
    document.body.appendChild(notification);

    setTimeout(() => {
        notification.style.animation = 'slideOutRight 0.3s ease-out';
        setTimeout(() => notification.remove(), 300);
    }, duration);
}

// ========================================
// DARK MODE TOGGLE (Optional Enhancement)
// ========================================

function initializeDarkMode() {
    // Check if dark mode is stored in localStorage
    const isDarkMode = localStorage.getItem('darkMode') !== 'false';
    
    if (isDarkMode) {
        document.body.classList.add('dark-mode');
    }
}

// ========================================
// INITIALIZE ALL FUNCTIONS ON PAGE LOAD
// ========================================

document.addEventListener('DOMContentLoaded', () => {
    console.log('🎯 BIG BET Website Loaded Successfully');
    
    // Initialize all features
    initializeMobileMenu();
    loadPredictions();
    loadResults();
    initializeScrollAnimations();
    initializeStatCounters();
    initializeSmoothScroll();
    updateActiveNavLink();
    initializeButtons();
    initializeDarkMode();

    // Add keyboard shortcut for admin panel (optional)
    document.addEventListener('keydown', (e) => {
        // Uncomment to enable: Ctrl+Shift+A for admin panel
        // if (e.ctrlKey && e.shiftKey && e.key === 'A') {
        //     console.log('Admin panel triggered');
        // }
    });
});

// ========================================
// PERFORMANCE OPTIMIZATION
// ========================================

// Lazy loading for images (if added later)
if ('IntersectionObserver' in window) {
    const imageObserver = new IntersectionObserver((entries, observer) => {
        entries.forEach(entry => {
            if (entry.isIntersecting) {
                const img = entry.target;
                img.src = img.dataset.src;
                img.classList.add('loaded');
                observer.unobserve(img);
            }
        });
    });

    document.querySelectorAll('img[data-src]').forEach(img => imageObserver.observe(img));
}

// ========================================
// SERVICE WORKER FOR OFFLINE SUPPORT (Optional)
// ========================================

if ('serviceWorker' in navigator) {
    window.addEventListener('load', () => {
        // Uncomment to enable service worker
        // navigator.serviceWorker.register('sw.js').catch(() => {});
    });
}

// ========================================
// ANALYTICS TRACKING (Optional - Add your tracking code)
// ========================================

// Example: Track page views
window.addEventListener('load', () => {
    // Add your analytics code here
    // Example: gtag('event', 'page_view');
});

// Track button clicks
document.addEventListener('click', (e) => {
    if (e.target.classList.contains('btn')) {
        const buttonText = e.target.textContent;
        // Add your analytics code here
        // Example: gtag('event', 'button_click', { button_text: buttonText });
    }
});

// ========================================
// UTILITY FUNCTIONS
// ========================================

// Format currency
function formatCurrency(value) {
    return new Intl.NumberFormat('en-US', {
        style: 'currency',
        currency: 'USD'
    }).format(value);
}

// Get current date formatted
function getCurrentDate() {
    const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
    return new Date().toLocaleDateString('en-US', options);
}

// Export functions for global use
window.BIG_BET = {
    showNotification,
    formatCurrency,
    getCurrentDate,
    predictionsData,
    resultsData
};

// Log initialization
console.log('✅ All modules initialized');
console.log('🎯 Website is ready for deployment');
