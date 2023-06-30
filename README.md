// Função para obter as estatísticas do GitHub
function getGithubStats(username) {
  const url = `https://api.github.com/users/${username}/repos`;

  fetch(url)
    .then(response => response.json())
    .then(data => {
      const totalCommits = data.reduce((acc, repo) => acc + repo.size, 0);
      const languages = {};

      // Contabiliza as linguagens usadas nos repositórios
      data.forEach(repo => {
        const language = repo.language;
        if (language) {
          if (languages[language]) {
            languages[language] += 1;
          } else {
            languages[language] = 1;
          }
        }
      });

      // Ordena as linguagens por quantidade de uso
      const sortedLanguages = Object.entries(languages).sort((a, b) => b[1] - a[1]);

      // Renderiza as estatísticas no gráfico
      renderStats(totalCommits, sortedLanguages);
    })
    .catch(error => console.log('Erro:', error));
}

// Função para renderizar as estatísticas no gráfico
function renderStats(totalCommits, languages) {
  const chartData = languages.map(language => ({
    label: language[0],
    value: language[1]
  }));

  // Renderiza o gráfico usando uma biblioteca como Chart.js ou Google Charts
  // Aqui está um exemplo usando Chart.js
  const ctx = document.getElementById('github-stats').getContext('2d');
  new Chart(ctx, {
    type: 'bar',
    data: {
      labels: chartData.map(data => data.label),
      datasets: [{
        label: 'Linguagens mais usadas',
        data: chartData.map(data => data.value),
        backgroundColor: 'rgba(75, 192, 192, 0.2)',
        borderColor: 'rgba(75, 192, 192, 1)',
        borderWidth: 1
      }]
    },
    options: {
      scales: {
        y: {
          beginAtZero: true,
          stepSize: 1
        }
      }
    }
  });
}

// Chame a função getGithubStats com seu nome de usuário do GitHub
getGithubStats('FelipeJustusMatos');
