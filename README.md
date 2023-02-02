# memo
### docker
1. `react_docker_environment`内のdockerfileを利用すれば簡易的なreact環境を作成可能　　
    1. docker compose build --no-cache
    2. npm install -g create-react-app && create-react-app react-todo-pwa-practice "(--template typescript)"
    3. docker compose exec react-app /bin/ash
        
