// Initialize data from localStorage
let matches = JSON.parse(localStorage.getItem('iplMatches')) || [];

const TEAMS = {
    'CSK': 'Chennai Super Kings',
    'MI': 'Mumbai Indians',
    'RCB': 'Royal Challengers Bangalore',
    'KKR': 'Kolkata Knight Riders',
    'DC': 'Delhi Capitals',
    'RR': 'Rajasthan Royals',
    'SRH': 'Sunrisers Hyderabad',
    'PBKS': 'Punjab Kings',
    'LSG': 'Lucknow Super Giants',
    'GT': 'Gujarat Titans'
};

// Tab switching
document.querySelectorAll('.tab-btn').forEach(btn => {
    btn.addEventListener('click', () => {
        const tabName = btn.getAttribute('data-tab');
        switchTab(tabName);
    });
});

function switchTab(tabName) {
    document.querySelectorAll('.tab-content').forEach(tab => {
        tab.classList.remove('active');
    });
    document.querySelectorAll('.tab-btn').forEach(btn => {
        btn.classList.remove('active');
    });

    document.getElementById(tabName).classList.add('active');
    document.querySelector(`[data-tab="${tabName}"]`).classList.add('active');

    if (tabName === 'results') {
        displayResults();
    } else if (tabName === 'standings') {
        displayStandings();
    }
}

// Form submission
document.getElementById('matchForm').addEventListener('submit', (e) => {
    e.preventDefault();

    const team1 = document.getElementById('team1').value;
    const team1Score = parseInt(document.getElementById('team1Score').value);
    const team2 = document.getElementById('team2').value;
    const team2Score = parseInt(document.getElementById('team2Score').value);
    const matchDate = document.getElementById('matchDate').value;
    const venue = document.getElementById('venue').value;

    if (team1 === team2) {
        alert('Please select different teams!');
        return;
    }

    const match = {
        id: Date.now(),
        team1,
        team1Score,
        team2,
        team2Score,
        date: matchDate,
        venue,
        winner: team1Score > team2Score ? team1 : team2
    };

    matches.unshift(match);
    localStorage.setItem('iplMatches', JSON.stringify(matches));

    document.getElementById('matchForm').reset();
    alert('Match added successfully!');
});

function displayResults() {
    const resultsList = document.getElementById('resultsList');

    if (matches.length === 0) {
        resultsList.innerHTML = '<p class="empty-message">No matches recorded yet.</p>';
        return;
    }

    resultsList.innerHTML = matches.map(match => `
        <div class="match-card">
            <div class="match-header">
                <div class="match-date">${new Date(match.date).toLocaleDateString('en-IN')}</div>
                <button class="btn btn-danger" onclick="deleteMatch(${match.id})">Delete</button>
            </div>
            <div class="teams-score">
                <div class="team">
                    <div class="team-name">${TEAMS[match.team1]}</div>
                    <div class="team-score">${match.team1Score}</div>
                </div>
                <div class="vs">VS</div>
                <div class="team">
                    <div class="team-name">${TEAMS[match.team2]}</div>
                    <div class="team-score">${match.team2Score}</div>
                </div>
            </div>
            <div style="text-align: center;">
                <span class="winner-badge">🏆 ${TEAMS[match.winner]} Won</span>
            </div>
            <div class="venue">📍 ${match.venue}</div>
        </div>
    `).join('');
}

function displayStandings() {
    const standingsBody = document.getElementById('standingsBody');
    const standings = {};

    // Initialize standings
    Object.keys(TEAMS).forEach(teamCode => {
        standings[teamCode] = {
            name: TEAMS[teamCode],
            matches: 0,
            wins: 0,
            losses: 0,
            points: 0
        };
    });

    // Calculate standings
    matches.forEach(match => {
        standings[match.team1].matches++;
        standings[match.team2].matches++;

        if (match.winner === match.team1) {
            standings[match.team1].wins++;
            standings[match.team1].points += 2;
            standings[match.team2].losses++;
        } else {
            standings[match.team2].wins++;
            standings[match.team2].points += 2;
            standings[match.team1].losses++;
        }
    });

    // Sort by points
    const sortedStandings = Object.values(standings)
        .filter(team => team.matches > 0)
        .sort((a, b) => b.points - a.points);

    if (sortedStandings.length === 0) {
        standingsBody.innerHTML = '<tr><td colspan="5" style="text-align: center; padding: 20px; color: #999;">No matches played yet.</td></tr>';
        return;
    }

    standingsBody.innerHTML = sortedStandings.map((team, index) => `
        <tr>
            <td><strong>${index + 1}. ${team.name}</strong></td>
            <td>${team.matches}</td>
            <td>${team.wins}</td>
            <td>${team.losses}</td>
            <td><strong>${team.points}</strong></td>
        </tr>
    `).join('');
}

function deleteMatch(id) {
    if (confirm('Are you sure you want to delete this match?')) {
        matches = matches.filter(match => match.id !== id);
        localStorage.setItem('iplMatches', JSON.stringify(matches));
        displayResults();
    }
}

// Initial display
displayResults();
