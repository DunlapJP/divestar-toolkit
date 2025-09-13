---
layout: default
title: Financial Deep Dive
---

<style>
/* Custom styles for the financial dashboard */
:root {
    --primary-green: #3a8c5b;
    --primary-red: #d94545;
    --primary-orange: #fd7e14;
    --dark-text: #212529;
    --light-text: #6c757d;
    --background-color: #f8f9fa;
    --card-background: #ffffff;
    --border-color: #dee2e6;
}

body {
    background-color: var(--background-color);
}

.dashboard-header {
    text-align: center;
    margin-bottom: 40px;
}

.dashboard-header h1 {
    font-size: 2.8rem;
    color: var(--dark-text);
    margin-bottom: 8px;
}

.dashboard-header .lead {
    font-size: 1.2rem;
    color: var(--light-text);
    max-width: 800px;
    margin: auto;
}

.dashboard-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 24px;
    margin-bottom: 24px;
}

.kpi-card {
    background-color: var(--card-background);
    border-radius: 8px;
    padding: 24px;
    box-shadow: 0 4px 8px rgba(0,0,0,0.05);
    border-top: 4px solid var(--primary-green);
}

.kpi-card.loss {
    border-top-color: var(--primary-red);
}
.kpi-card.auth {
    border-top-color: var(--primary-orange);
}


.kpi-card h3 {
    margin: 0 0 8px 0;
    font-size: 1rem;
    color: var(--light-text);
    font-weight: 600;
}

.kpi-card .value {
    font-size: 2.2rem;
    font-weight: 700;
    color: var(--dark-text);
}

.kpi-card .value.loss-value {
    color: var(--primary-red);
}

.content-card {
    background-color: var(--card-background);
    border-radius: 8px;
    padding: 24px;
    box-shadow: 0 4px 8px rgba(0,0,0,0.05);
    margin-bottom: 24px;
}

.content-card h3 {
    margin-top: 0;
    font-size: 1.4rem;
    font-weight: 600;
    color: #343a40;
}

.bar-chart-container {
    padding-top: 20px;
}

.bar-chart-item {
    display: flex;
    align-items: center;
    margin-bottom: 12px;
}

.bar-chart-item .label {
    width: 25%;
    font-size: 0.9rem;
    color: var(--light-text);
    padding-right: 10px;
}

.bar-chart-item .bar-wrapper {
    width: 75%;
    background-color: #e9ecef;
    border-radius: 4px;
    overflow: hidden;
}

.bar-chart-item .bar {
    height: 24px;
    line-height: 24px;
    color: white;
    font-weight: 600;
    font-size: 0.85rem;
    text-align: right;
    padding-right: 8px;
    white-space: nowrap;
}

.table-container {
    overflow-x: auto;
}

#portfolioTable {
    width: 100%;
    border-collapse: collapse;
}

#portfolioTable th, #portfolioTable td {
    padding: 12px 15px;
    text-align: left;
    border-bottom: 1px solid var(--border-color);
}

#portfolioTable th {
    background-color: #f8f9fa;
    font-weight: 600;
    color: #495057;
}

#portfolioTable tbody tr:hover {
    background-color: #f1f3f5;
}

#portfolioTable .loss-value {
    color: var(--primary-red);
    font-weight: 500;
}

.risk-section {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 24px;
}

.risk-card {
    background-color: #fff5f5;
    border-left: 5px solid var(--primary-red);
    border-radius: 8px;
    padding: 24px;
}

.risk-card h4 {
    margin-top: 0;
    color: #c53030;
}

.data-source-note {
    text-align: center;
    font-size: 0.85em;
    color: var(--light-text);
    margin-top: 20px;
}

.sparkline-bar-chart {
    display: flex;
    align-items: flex-end;
    justify-content: space-between;
    height: 150px;
    padding: 10px 0;
    border-bottom: 2px solid var(--border-color);
}

.sparkline-bar {
    width: 8%;
    background-color: var(--primary-green);
    position: relative;
    transition: background-color 0.3s;
}

.sparkline-bar.negative {
    background-color: var(--primary-red);
}

.sparkline-bar:hover {
    opacity: 0.8;
}

.sparkline-bar .tooltip {
    visibility: hidden;
    width: 80px;
    background-color: var(--dark-text);
    color: #fff;
    text-align: center;
    border-radius: 6px;
    padding: 5px 0;
    position: absolute;
    z-index: 1;
    bottom: 105%;
    left: 50%;
    margin-left: -40px;
    opacity: 0;
    transition: opacity 0.3s;
}

.sparkline-bar:hover .tooltip {
    visibility: visible;
    opacity: 1;
}

.sparkline-labels {
    display: flex;
    justify-content: space-between;
    font-size: 0.8rem;
    color: var(--light-text);
    padding: 5px 0;
}
</style>

<div class="dashboard-header">
    <h1>Financial Deep Dive</h1>
    <p class="lead">An analysis of Arkansas's public investments in Israel Bonds, portfolio performance, and market context.</p>
</div>

<div class="dashboard-grid">
    <div class="kpi-card">
        <h3>Total Public Commitment</h3>
        <div class="value">$157 M</div>
    </div>
    <div class="kpi-card">
        <h3>State Treasury Invested</h3>
        <div class="value">$57 M</div>
    </div>
    <div class="kpi-card loss">
        <h3>Unrealized Loss (10/07/2024)</h3>
        <div class="value loss-value">-$1.2 M</div>
    </div>
    <div class="kpi-card auth">
        <h3>Pension Funds Authorized</h3>
        <div class="value">$100 M</div>
    </div>
</div>

<div class="content-card">
    <h3>Commitment Breakdown by Source</h3>
    <div class="bar-chart-container">
        <div class="bar-chart-item">
            <div class="label">AR State Treasury</div>
            <div class="bar-wrapper">
                <div class="bar" style="width: 36.3%; background-color: var(--primary-green);"> $57M</div>
            </div>
        </div>
        <div class="bar-chart-item">
            <div class="label">ATRS (Authorized)</div>
            <div class="bar-wrapper">
                <div class="bar" style="width: 31.8%; background-color: var(--primary-orange);">$50M</div>
            </div>
        </div>
        <div class="bar-chart-item">
            <div class="label">APERS (Authorized)</div>
            <div class="bar-wrapper">
                <div class="bar" style="width: 31.8%; background-color: var(--primary-red);">$50M</div>
            </div>
        </div>
    </div>
</div>

<div class="content-card">
    <h3>Market Proxy: S&P Israel Sovereign Bond Index (Annual Total Return %)</h3>
    <div id="calendarYearChart">
        <div class="sparkline-bar-chart">
            </div>
        <div class="sparkline-labels">
            </div>
    </div>
    <p class="data-source-note">This chart shows the historical performance of a market index and is for illustrative purposes. It does not represent the exact performance of bonds held by Arkansas.<br>Source: S&P Dow Jones Indices. Data from 2015-2024.</p>
</div>

<div class="table-container">
    <h3>AR State Treasury Portfolio Details (10/07/2024)</h3>
    <table id="portfolioTable">
        <thead>
            <tr>
                <th>Description</th>
                <th>Coupon</th>
                <th>Maturity</th>
                <th>Original Cost</th>
                <th>Market Value</th>
                <th>Unrealized Gain/Loss</th>
            </tr>
        </thead>
        <tbody>
            </tbody>
    </table>
    <p class="data-source-note">Source: Arkansas State Treasury, Portfolio Holdings Report, Oct. 7, 2024.</p>
</div>

<div style="margin-top: 40px; margin-bottom: 40px;">
    <h2 style="text-align: center; color: var(--dark-text); font-weight: 600;">Risk Assessment: Credit Rating Downgrades</h2>
    <div class="risk-section" style="margin-top: 20px;">
        <div class="risk-card">
            <h4>Moody's Downgrade to A2</h4>
            <p>On February 9, 2024, Moody's downgraded Israel's credit rating, citing "heightened geopolitical risks and a negative outlook." This indicates a higher perceived risk for investors.</p>
        </div>
        <div class="risk-card">
            <h4>S&P Global Downgrade to A</h4>
            <p>On October 1, 2024, S&P Global lowered Israel's rating due to "heightened security risks and a negative outlook," further signaling increased financial uncertainty for bondholders.</p>
        </div>
    </div>
</div>

<script>
document.addEventListener("DOMContentLoaded", function() {

    // Helper function for currency formatting
    const currencyFormatter = new Intl.NumberFormat('en-US', { style: 'currency', currency: 'USD' });

    // Populate Calendar Year Performance Chart
    const annualReturnsData = [
        { year: '2015', value: 3.21 }, { year: '2016', value: 1.30 }, { year: '2017', value: 4.01 }, 
        { year: '2018', value: -1.18 }, { year: '2019', value: 9.58 }, { year: '2020', value: 1.46 }, 
        { year: '2021', value: -0.92 }, { year: '2022', value: -10.07 }, { year: '2023', value: 1.48 }, 
        { year: '2024', value: 2.51 }
    ];
    const chartContainer = document.querySelector("#calendarYearChart .sparkline-bar-chart");
    const labelsContainer = document.querySelector("#calendarYearChart .sparkline-labels");
    const maxValue = Math.max(...annualReturnsData.map(d => Math.abs(d.value)));

    annualReturnsData.forEach(data => {
        const bar = document.createElement('div');
        bar.className = 'sparkline-bar';
        if (data.value < 0) {
            bar.classList.add('negative');
        }
        bar.style.height = `${(Math.abs(data.value) / maxValue) * 100}%`;
        
        const tooltip = document.createElement('span');
        tooltip.className = 'tooltip';
        tooltip.textContent = `${data.value.toFixed(2)}%`;
        bar.appendChild(tooltip);
        
        chartContainer.appendChild(bar);

        const label = document.createElement('div');
        label.textContent = data.year;
        labelsContainer.appendChild(label);
    });

    // Fetch and process Treasury CSV data for the table
    const treasuryFile = '{{ "/source-documents/2024-10-07_AR-Treasury_Portfolio-Holdings_Israel-Bonds.xlsx - Israel Bonds.csv" | relative_url }}';
    Papa.parse(treasuryFile, {
        download: true,
        header: true,
        complete: function(results) {
            const bondData = results.data.slice(0, -1); // Remove summary row

            // Populate Portfolio Table
            const tableBody = document.querySelector("#portfolioTable tbody");
            bondData.forEach(bond => {
                let row = tableBody.insertRow();
                let lossValue = parseFloat(bond['Base Net Total Unrealized Gain/Loss']);
                row.insertCell().textContent = bond.Description;
                row.insertCell().textContent = (parseFloat(bond['Coupon Rate']) * 100).toFixed(2) + '%';
                row.insertCell().textContent = bond['Final Maturity'];
                row.insertCell().textContent = currencyFormatter.format(bond['Base Original Cost']);
                row.insertCell().textContent = currencyFormatter.format(bond['Base Market Value']);
                const lossCell = row.insertCell();
                lossCell.textContent = currencyFormatter.format(lossValue);
                if (lossValue < 0) {
                    lossCell.className = 'loss-value';
                }
            });
        }
    });
});
</script>
