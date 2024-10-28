# simulation-modling
possion distribution complete flow and working strategies
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Poisson Distribution Calculator</title>
</head>
<body>

    <h1>Poisson Distribution Calculator</h1>
    <label for="lambdaInput">Enter the average number of accidents per week (lambda):</label>
    <input type="number" id="lambdaInput" step="0.1" min="0" required>
    <button id="calculateButton">Calculate</button>

    <h2>Results:</h2>
    <pre id="results"></pre>

    <script>
        class Poisson {
            constructor(lambda) {
                this.lambda = lambda;
            }

            factorial(n) {
                if (n === 0 || n === 1) return 1;
                let result = 1;
                for (let i = 2; i <= n; i++) {
                    result *= i;
                }
                return result;
            }

            poissonProbability(k) {
                return (Math.exp(-this.lambda) * Math.pow(this.lambda, k)) / this.factorial(k);
            }

            probabilityLessThan(k) {
                let probability = 0;
                for (let i = 0; i < k; i++) {
                    probability += this.poissonProbability(i);
                }
                return probability;
            }

            probabilityMoreThan(k) {
                return 1 - this.probabilityLessThan(k + 1);
            }
        }

        document.getElementById('calculateButton').addEventListener('click', () => {
            const lambdaInput = parseFloat(document.getElementById('lambdaInput').value);
            if (isNaN(lambdaInput) || lambdaInput < 0) {
                alert('Please enter a valid positive number for lambda.');
                return;
            }

            const poisson = new Poisson(lambdaInput);

            // Calculate probabilities for part (a)
            const results = [];
            results.push(`Part (a):`);
            results.push(`1. Probability of less than 2 accidents: ${poisson.probabilityLessThan(2).toFixed(4)}`);
            results.push(`2. Probability of more than 2 accidents: ${poisson.probabilityMoreThan(2).toFixed(4)}`);

            // Calculate probabilities for part (b)
            const threeWeekLambda = lambdaInput * 3; // Mean for 3 weeks
            const poissonThreeWeeks = new Poisson(threeWeekLambda);
            const noAccidentsThreeWeeks = poissonThreeWeeks.poissonProbability(0);

            results.push(`Part (b):`);
            results.push(`Probability of no accidents in 3 weeks: ${noAccidentsThreeWeeks.toFixed(4)}`);

            document.getElementById('results').textContent = results.join('\n');
        });
    </script>

</body>
</html>
