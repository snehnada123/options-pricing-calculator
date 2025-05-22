# options-pricing-calculator
created a options pricing calculator along with payoff graph for a better understanding
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Option Greek Calculator - Indian Markets</title>
<style>
  /* Black and white theme styling */
  body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background: #121212;
    margin: 0;
    padding: 0;
    color: #EEEEEE;
    overflow-x: hidden;
    position: relative;
    min-height: 100vh;
  }
  main {
    background: #1E1E1E;
    border-radius: 12px;
    padding: 2rem;
    box-shadow: 0 8px 24px rgba(255,255,255,0.1);
    max-width: 700px;
    width: 90%;
    margin: 4rem auto 5rem auto;
    position: relative;
    z-index: 10;
  }
  h1 {
    text-align: center;
    color: #FFFFFF;
    margin-bottom: 1.2rem;
  }
  label {
    display: block;
    margin-top: 1rem;
    font-weight: 600;
    color: #DDDDDD;
  }
  input[type="number"] {
    margin-top: 0.3rem;
    padding: 0.5rem;
    border: 2px solid #444;
    border-radius: 6px;
    font-size: 1rem;
    width: 100%;
    box-sizing: border-box;
    background: #2A2A2A;
    color: #EEE;
    transition: border-color 0.3s ease;
  }
  input[type="number"]::placeholder {
    color: #BBB;
  }
  input[type="number"]:focus {
    border-color: #BBBBBB;
    outline: none;
    background: #333;
  }
  button {
    margin-top: 1.8rem;
    width: 100%;
    padding: 0.75rem;
    font-size: 1.1rem;
    font-weight: 700;
    border: none;
    background: #FFFFFF;
    color: #121212;
    border-radius: 8px;
    cursor: pointer;
    box-shadow: 0 5px 12px rgba(255,255,255,0.6);
    transition: background-color 0.3s ease, color 0.3s ease;
  }
  button:hover {
    background: #DDDDDD;
  }
  /* Tabs container */
  .tab-container {
    margin-top: 2rem;
    border-top: 1px solid #444;
    display: none; /* hidden initially */
  }
  .tabs {
    display: flex;
    border-bottom: 1px solid #444;
  }
  .tab {
    flex: 1;
    text-align: center;
    padding: 0.6rem 0;
    cursor: pointer;
    color: #AAA;
    font-weight: 600;
    user-select: none;
    background: #2a2a2a;
    border-top-left-radius: 8px;
    border-top-right-radius: 8px;
  }
  .tab.active {
    color: #FFFFFF;
    background: #1e1e1e;
    border-bottom: 1px solid #1e1e1e;
  }
  .tab.disabled {
    cursor: not-allowed;
    color: #555;
  }
  .tab-content {
    background: #333333;
    padding: 1.5rem;
    border-radius: 0 0 8px 8px;
    color: #FFFFFF;
  }
  #result {
    font-size: 1.2rem;
    text-align: center;
    user-select: all;
  }
  #payoffChart {
    max-width: 100%;
    height: 300px;
  }
  .greek-info {
    margin-top: 2.5rem;
    border-top: 1px solid #444;
    padding-top: 2rem;
  }
  .greek {
    margin-bottom: 1.6rem;
  }
  .greek h3 {
    color: #FFFFFF;
    margin-bottom: 0.3rem;
  }
  .greek p {
    margin: 0.2rem 0;
    line-height: 1.4;
    color: #CCCCCC;
  }
  .example {
    font-style: italic;
    color: #AAAAAA;
    background: #222222;
    padding: 0.5rem 1rem;
    border-left: 4px solid #FFFFFF;
    margin-top: 0.3rem;
    border-radius: 4px;
  }
  @media (max-width: 480px) {
    main {
      padding: 1rem 1.2rem;
      margin: 2rem auto 3rem auto;
      width: 95%;
    }
  }

  /* Animated blob background similar to Cred style */
  .blob {
    position: fixed;
    border-radius: 50%;
    filter: blur(120px);
    opacity: 0.25;
    animation-timing-function: ease-in-out;
    animation-iteration-count: infinite;
    z-index: 0;
    pointer-events: none;
  }
  .blob1 {
    width: 300px;
    height: 300px;
    background: radial-gradient(circle at 40% 40%, #ffffff, transparent 60%);
    top: 10vh;
    left: 5vw;
    animation-name: blobMove1;
    animation-duration: 35s;
    animation-direction: alternate;
  }
  .blob2 {
    width: 350px;
    height: 350px;
    background: radial-gradient(circle at 60% 60%, #bbbbbb, transparent 65%);
    bottom: 15vh;
    right: 10vw;
    animation-name: blobMove2;
    animation-duration: 40s;
    animation-direction: alternate-reverse;
  }
  .blob3 {
    width: 250px;
    height: 250px;
    background: radial-gradient(circle at 50% 50%, #dddddd, transparent 70%);
    top: 50vh;
    right: 50vw;
    animation-name: blobMove3;
    animation-duration: 30s;
    animation-direction: alternate;
  }
  @keyframes blobMove1 {
    0% { transform: translate(0, 0) scale(1);}
    50% { transform: translate(30px, -20px) scale(1.1);}
    100% { transform: translate(0, 0) scale(1);}
  }
  @keyframes blobMove2 {
    0% { transform: translate(0, 0) scale(1);}
    50% { transform: translate(-25px, 15px) scale(1.1);}
    100% { transform: translate(0, 0) scale(1);}
  }
  @keyframes blobMove3 {
    0% { transform: translate(0, 0) scale(1);}
    50% { transform: translate(-20px, -15px) scale(1.05);}
    100% { transform: translate(0, 0) scale(1);}
  }
</style>
</head>
<body>
  <div class="blob blob1" aria-hidden="true"></div>
  <div class="blob blob2" aria-hidden="true"></div>
  <div class="blob blob3" aria-hidden="true"></div>

<main>
  <h1>OPTION GREEK CALCULATOR - BY SNEH NADA</h1>
  <form id="greekCalcForm" aria-label="Option Greek Calculator Form">
    <label for="currentPrice">Current Price of Underlying (₹)</label>
    <input type="number" step="0.01" id="currentPrice" name="currentPrice" required min="0" placeholder="e.g., 17500" aria-describedby="currentPriceHelp" />
    <small id="currentPriceHelp" style="color:#BBB;">Price of the futures or underlying asset</small>

    <label for="delta">
      Delta (β) <span aria-hidden="true">(Sensitivity to Price)</span>
    </label>
    <input type="number" step="0.0001" id="delta" name="delta" required placeholder="e.g., 0.5" aria-describedby="deltaHelp" />
    <small id="deltaHelp" style="color:#BBB;">Change in option price for a ₹1 change in underlying</small>

    <label for="gamma">
      Gamma (γ) <span aria-hidden="true">(Rate of Change of Delta)</span>
    </label>
    <input type="number" step="0.0001" id="gamma" name="gamma" required placeholder="e.g., 0.05" aria-describedby="gammaHelp" />
    <small id="gammaHelp" style="color:#BBB;">How much the delta changes if underlying price changes by ₹1</small>

    <label for="theta">
      Theta (θ) <span aria-hidden="true">(Time Decay)</span>
    </label>
    <input type="number" step="0.01" id="theta" name="theta" required placeholder="e.g., -12" aria-describedby="thetaHelp" />
    <small id="thetaHelp" style="color:#BBB;">Change in option price per day, due to time passing</small>

    <label for="vega">
      Vega (ν) <span aria-hidden="true">(Volatility Sensitivity)</span>
    </label>
    <input type="number" step="0.01" id="vega" name="vega" required placeholder="e.g., 20" aria-describedby="vegaHelp" />
    <small id="vegaHelp" style="color:#BBB;">Change in option price for 1% change in implied volatility</small>

    <label for="days">Days to Expiry</label>
    <input type="number" step="1" id="days" name="days" required min="0" placeholder="e.g., 15" aria-describedby="daysHelp" />
    <small id="daysHelp" style="color:#BBB;">Number of days left until option expiry</small>

    <label for="ivChange">Expected Change in Volatility (%)</label>
    <input type="number" step="0.01" id="ivChange" name="ivChange" required placeholder="e.g., 2" aria-describedby="ivHelp" />
    <small id="ivHelp" style="color:#BBB;">How much implied volatility is expected to change (%)</small>

    <label for="priceChange">Expected Change in Underlying Price (₹)</label>
    <input type="number" step="0.01" id="priceChange" name="priceChange" required placeholder="e.g., 100" aria-describedby="priceChangeHelp" />
    <small id="priceChangeHelp" style="color:#BBB;">Expected price movement of the underlying asset</small>

    <label for="currentOptionPrice">Current Option Price (₹)</label>
    <input type="number" step="0.01" id="currentOptionPrice" name="currentOptionPrice" required placeholder="e.g., 150" aria-describedby="currentOptionPriceHelp" />
    <small id="currentOptionPriceHelp" style="color:#BBB;">Current price of the option or futures contract</small>

    <button type="submit" aria-label="Calculate predicted option price">
      Calculate Predicted Price
    </button>
  </form>

  <!-- Tabs container, hidden by default -->
  <div class="tab-container" id="tabsContainer" aria-label="Calculation results tabs">
    <div class="tabs" role="tablist">
      <div class="tab active" id="resultTab" role="tab" tabindex="0" aria-selected="true" aria-controls="resultContent">Result</div>
      <div class="tab disabled" id="payoffTab" role="tab" tabindex="-1" aria-selected="false" aria-controls="payoffContent">Payoff Graph</div>
    </div>
    <div class="tab-content" id="resultContent" role="tabpanel" aria-labelledby="resultTab">
      <div id="result" style="font-size:1.2rem; user-select:all;">&nbsp;</div>
    </div>
    <div class="tab-content" id="payoffContent" role="tabpanel" aria-labelledby="payoffTab" style="display:none;">
      <canvas id="payoffChart" style="max-width:100%; height:300px;"></canvas>
    </div>
  </div>

  <section class="greek-info" aria-label="Greek definitions and examples">
    <h2>Option Greeks - Definitions &amp; Examples</h2>

    <article class="greek" id="delta-info">
      <h3>Delta (β)</h3>
      <p>
        Delta represents how much the price of an option will change for a ₹1
        change in the underlying asset’s price.
      </p>
      <p class="example">
        Example: A delta of 0.5 means if the stock rises ₹1, the option price
        will rise about ₹0.50.
      </p>
    </article>

    <article class="greek" id="gamma-info">
      <h3>Gamma (γ)</h3>
      <p>Gamma shows the rate of change of delta when the underlying price changes by ₹1.</p>
      <p class="example">
        Example: Gamma of 0.05 means if underlying price increases ₹1, delta
        will increase by 0.05.
      </p>
    </article>

    <article class="greek" id="theta-info">
      <h3>Theta (θ)</h3>
      <p>
        Theta measures how much the option price changes as time passes, assuming all else stays the same. It usually decreases option value.
      </p>
      <p class="example">
        Example: Theta of -12 means the option loses ₹12 in value per day due to time decay.
      </p>
    </article>

    <article class="greek" id="vega-info">
      <h3>Vega (ν)</h3>
      <p>
        Vega measures sensitivity of the option price to a change in implied volatility of 1%.
      </p>
      <p class="example">
        Example: Vega of 20 means if volatility rises by 1%, option price increases by ₹20.
      </p>
    </article>
  </section>
</main>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script>
  let payoffChartInstance = null;

  // Utility function to switch tabs
  function switchTab(tabId) {
    const tabs = document.querySelectorAll('.tab');
    const tabContents = document.querySelectorAll('.tab-content');
    tabs.forEach(tab => {
      const isActive = tab.id === tabId;
      tab.classList.toggle('active', isActive);
      tab.setAttribute('aria-selected', isActive);
      tab.tabIndex = isActive ? 0 : -1;
    });
    tabContents.forEach(content => {
      content.style.display = content.id === tabId.replace('Tab','Content') ? 'block' : 'none';
    });
  }

  // Setup tab click and keyboard accessibility
  document.getElementById('resultTab').addEventListener('click', () => switchTab('resultTab'));
  document.getElementById('payoffTab').addEventListener('click', () => {
    if (!document.getElementById('payoffTab').classList.contains('disabled')) {
      switchTab('payoffTab');
    }
  });
  // Keyboard navigation on tabs
  document.querySelectorAll('.tab').forEach(tab => {
    tab.addEventListener('keydown', e => {
      if (e.key === 'ArrowRight' || e.key === 'ArrowLeft') {
        e.preventDefault();
        const enabledTabs = Array.from(document.querySelectorAll('.tab:not(.disabled)'));
        const currentIndex = enabledTabs.indexOf(tab);
        let newIndex = currentIndex + (e.key === 'ArrowRight' ? 1 : -1);
        if (newIndex < 0) newIndex = enabledTabs.length -1;
        if (newIndex >= enabledTabs.length) newIndex = 0;
        switchTab(enabledTabs[newIndex].id);
        enabledTabs[newIndex].focus();
      }
    });
  });

  document.getElementById('greekCalcForm').addEventListener('submit', function(event) {
    event.preventDefault();

    // Fetch values and parse
    const currentPrice = parseFloat(this.currentPrice.value);
    const delta = parseFloat(this.delta.value);
    const gamma = parseFloat(this.gamma.value);
    const theta = parseFloat(this.theta.value);
    const vega = parseFloat(this.vega.value);
    const days = parseInt(this.days.value);
    const ivChangePct = parseFloat(this.ivChange.value);
    const priceChange = parseFloat(this.priceChange.value);
    const currentOptionPrice = parseFloat(this.currentOptionPrice.value);

    // Validate inputs
    if (isNaN(currentPrice) || isNaN(delta) || isNaN(gamma) || isNaN(theta) || isNaN(vega) || isNaN(days) || isNaN(ivChangePct) || isNaN(priceChange) || isNaN(currentOptionPrice)) {
      alert('Please enter valid numbers for all fields.');
      return;
    }

    // Convert volatility change % to decimal
    const ivChange = ivChangePct / 100;

    // Calculate price change
    const priceChangeSquared = priceChange * priceChange;

    const optionPriceChange = (delta * priceChange) 
                            + (0.5 * gamma * priceChangeSquared) 
                            + (theta * days) 
                            + (vega * ivChange);

    const predictedPrice = currentOptionPrice + optionPriceChange;

    // Format to 2 decimals and clamp predicted price to not go below zero
    const finalPrice = Math.max(0, predictedPrice).toFixed(2);

    // Show result tab and container
    document.getElementById('tabsContainer').style.display = 'block';
    switchTab('resultTab');

    // Display result
    document.getElementById('result').textContent = `Predicted Option/Future Price: ₹${finalPrice}`;

    // Draw payoff graph
    drawPayoffGraph(currentPrice, finalPrice, delta, gamma, theta, vega);

    // Enable payoff tab after calculation
    const payoffTab = document.getElementById('payoffTab');
    payoffTab.classList.remove('disabled');
    payoffTab.tabIndex = 0;
  });

  function drawPayoffGraph(currentPrice, predictedPrice, delta, gamma, theta, vega) {
    const ctx = document.getElementById('payoffChart').getContext('2d');

    // Define a range of underlying prices around current price
    const priceRange = [];
    const payoffData = [];

    const step = 10;
    const startPrice = Math.max(currentPrice - 200, 0);
    const endPrice = currentPrice + 200;

    for(let price = startPrice; price <= endPrice; price += step) {
      priceRange.push(price);

      // Simplified payoff approximate model:
      // Payoff = intrinsic value - cost
      // Here we consider payoff = option price change approximated by Greeks + intrinsic payoff

      // Intrinsic payoff: if price > currentPrice, payoff is price - currentPrice (call option)
      // If price < currentPrice, payoff zero
      const intrinsic = Math.max(price - currentPrice, 0);

      // Option price change approximation by Greek sensitivities on price difference
      const priceDiff = price - currentPrice;
      const priceChange = priceDiff;
      const priceChangeSq = priceDiff * priceDiff;
      const optionPriceChange = delta * priceChange + 0.5 * gamma * priceChangeSq ;

      // Option value approx
      const optionValue = predictedPrice;

      // Net payoff = intrinsic - optionValue + optionPriceChange (to approximate how option value changes with price)
      const netPayoff = intrinsic - optionValue + optionPriceChange;

      payoffData.push(netPayoff.toFixed(2));
    }

    // Destroy previous chart if exists
    if (payoffChartInstance) {
      payoffChartInstance.destroy();
    }

    payoffChartInstance = new Chart(ctx, {
      type: 'line',
      data: {
        labels: priceRange,
        datasets: [{
          label: 'Payoff',
          data: payoffData,
          borderColor: 'rgba(255, 255, 255, 1)',
          backgroundColor: 'rgba(255, 255, 255, 0.25)',
          fill: true,
          tension: 0.15,
          borderWidth: 2,
          pointRadius: 0,
        }]
      },
      options: {
        responsive: true,
        animation: {
          duration: 500
        },
        scales: {
          x: {
            title: {
              display: true,
              text: 'Underlying Price (₹)',
              color: '#FFF',
            },
            ticks: {
              color: '#DDD'
            },
            grid: {
              color: '#444'
            }
          },
          y: {
            title: {
              display: true,
              text: 'Payoff (₹)',
              color: '#FFF',
            },
            ticks: {
              color: '#DDD'
            },
            grid: {
              color: '#444'
            }
          }
        },
        plugins: {
          legend: {
            labels: {
              color: '#FFF',
              font: {
                size: 14
              }
            }
          },
          tooltip: {
            enabled: true,
            mode: 'nearest',
            intersect: false,
            callbacks: {
              label: function(context) {
                return `Payoff: ₹${context.parsed.y}`;
              }
            }
          }
        }
      }
    });
  }
</script>
</body>
</html>
