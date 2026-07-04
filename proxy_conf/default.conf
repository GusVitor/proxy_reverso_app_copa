worker_processes auto;

events {
    worker_connections 1024;
}

http {

    ##
    ## Upstreams
    ##

    upstream america {
        server 10.0.1.10:80;
    }

    upstream europa {
        server 10.0.2.10:80;
    }

    upstream africa {
        server 10.0.3.10:80;
    }

    upstream asia {
        server 10.0.4.10:80;
    }

    ##
    ## Página principal
    ##

    server {
        listen 80;
        server_name ~^(?<ip>.+)\.nip\.io$;

        # Se existir subdomínio (am, eu, etc.), envia para o bloco abaixo
        if ($host ~ "^(am|eu|af|as)\..+\.nip\.io$") {
            return 444;
        }

        root /usr/share/nginx/html;
        index index.html;

        location / {
            try_files $uri $uri/ =404;
        }

        error_page 444 = @proxy;
    }

    ##
    ## Proxy reverso por região
    ##

    server {
        listen 80;

        server_name
            ~^am\..+\.nip\.io$
            ~^eu\..+\.nip\.io$
            ~^af\..+\.nip\.io$
            ~^as\..+\.nip\.io$;

        set $backend "";

        if ($host ~ "^am\.") {
            set $backend http://america;
        }

        if ($host ~ "^eu\.") {
            set $backend http://europa;
        }

        if ($host ~ "^af\.") {
            set $backend http://africa;
        }

        if ($host ~ "^as\.") {
            set $backend http://asia;
        }

        location / {
            proxy_pass $backend;

            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;

            proxy_http_version 1.1;
            proxy_set_header Connection "";
        }
    }

}