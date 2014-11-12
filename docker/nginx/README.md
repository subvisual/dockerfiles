# Testing this container

    docker build -t naps62/demo-nginx .
    # wait
    docker run --name demo-nginx -p 80:80 -i -t naps62/demo-nginx
