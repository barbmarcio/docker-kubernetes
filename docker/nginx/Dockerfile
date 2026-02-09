# Dockerfile para Nginx - Web Solutions Ltda.
# Imagem otimizada baseada em Alpine Linux

FROM nginx:alpine

# Metadados da imagem
LABEL maintainer="Web Solutions Ltda."
LABEL description="Servidor Nginx otimizado para Web Solutions"
LABEL version="1.0"

# Copiar página HTML personalizada
COPY index.html /usr/share/nginx/html/index.html

# Expor porta 80 (padrão do Nginx)
EXPOSE 80

# Comando para iniciar o Nginx em foreground
CMD ["nginx", "-g", "daemon off;"]
