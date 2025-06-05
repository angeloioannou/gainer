<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>🚀 Crypto Trade Edge</title>
  <style>
    body { font-family: sans-serif; background: #0b0c10; color: #fff; padding: 1em; }
    h1 { color: #00ffe1; }
    .coin { margin: 1em 0; padding: 1em; background: #1f2833; border-radius: 8px; }
    .alert { color: #ff3860; font-weight: bold; }
  </style>
</head>
<body>
  <h1>🚀 Crypto Trade Edge</h1>
  <div id="output">Loading data...</div>

  <script>
    const coins = ['bitcoin', 'ethereum', 'solana', 'pepe', 'dogecoin'];

    async function fetchCoinData(coin) {
      const res = await fetch(`https://api.coingecko.com/api/v3/coins/${coin}`);
      const json = await res.json();
      const price = json.market_data.current_price.usd;
      const high = json.market_data.high_24h.usd;
      const low = json.market_data.low_24h.usd;
      const change = json.market_data.price_change_percentage_24h;
      return { name: coin.toUpperCase(), price, high, low, change };
    }

    async function getListings() {
      const res = await fetch("https://listingspy.net/api/new"); // Hypothetical API
      const listings = await res.json();
      return listings.slice(0, 5).map(c => c.name);
    }

    async function render() {
      const output = document.getElementById("output");
      output.innerHTML = '';

      for (const coin of coins) {
        const data = await fetchCoinData(coin);
        const alert = Math.abs(data.change) >= 80 ? '<span class="alert">🔥 80%+ Move</span>' : '';
        output.innerHTML += `
          <div class="coin">
            <h2>${data.name} ${alert}</h2>
            <p>Price: $${data.price.toFixed(2)} | High: $${data.high.toFixed(2)} | Low: $${data.low.toFixed(2)}</p>
            <p>24h Change: ${data.change.toFixed(2)}%</p>
          </div>
        `;
      }

      try {
        const newListings = await getListings();
        output.innerHTML += `<h3>🆕 New Listings</h3><ul>${newListings.map(c => `<li>${c}</li>`).join('')}</ul>`;
      } catch (e) {
        output.innerHTML += `<p style="color:gray;">New listings unavailable</p>`;
      }
    }

    render();
    setInterval(render, 60000); // Refresh every 1 minute
  </script>
</body>
</html>
