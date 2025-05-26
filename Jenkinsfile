pipeline {
    agent any
    environment {
        TEMP = sh(returnStdout: true, script: 'curl -s "https://api.open-meteo.com/v1/forecast?latitude=-1.4761&longitude=36.9614&daily=temperature_2m_max&timezone=Africa%2FCairo&start_date=2025-05-26&end_date=2025-05-26" | jq -r .daily.temperature_2m_max[0]').trim()
        RATE = sh(returnStdout: true, script: 'curl -s "https://open.er-api.com/v6/latest/USD" | jq -r .rates.KES').trim()
    }
    parameters {
        string defaultValue: '30', description: 'Maximum temperature', name: 'MAX_TEMP'
        string defaultValue: '150', description: 'Maximum exchange rate', name: 'MAX_RATE'
    }
    stages {
        stage('Temperature') {
            steps {
                script {
                    def temp = env.TEMP.toFloat()
                    def maxTemp = params.MAX_TEMP.toFloat()
                    if (temp > maxTemp) {
                        echo "Too hot! Temp: ${temp}°C"
                        error("Temperature too high too high ughhhhhhhh")
                    } else {
                        echo "Good Temp. Temp: ${temp}°C"
                    }
                }
            }
        }
        stage('Rate') {
            steps {
                script {
                    def rate = env.RATE.toFloat()
                    def maxRate = params.MAX_RATE.toFloat()
                    if (rate > maxRate) {
                        echo "KES rate is bad! Rate: ${rate} KES/USD"
                        error("Exchange rate too high")
                    } else {
                        echo "KES rate is good. Rate: ${rate} KES/USD"
                    }
                }
            }
        }
    }
}
