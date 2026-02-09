# Dockerfile para Apache HTTPD - Web Solutions Ltda.
# Imagem otimizada baseada em Alpine Linux

FROM httpd:alpine

# Metadados da imagem
LABEL maintainer="Web Solutions Ltda."
LABEL description="Servidor Apache HTTPD otimizado para Web Solutions"
LABEL version="1.0"

# Copiar página HTML personalizada
COPY index.html /usr/local/apache2/htdocs/index.html

# Expor porta 80 (padrão do Apache)
EXPOSE 80

# O comando padrão já é executar o Apache em foreground
