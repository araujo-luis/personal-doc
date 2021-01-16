# Dockerfile

You can use `\` to separate commands in different lines and `&&` when listing differents commands one after other, specially when you want to avoid many Docker layers.

	FROM required
	ENV Your environment variables
	WORKDIR Change your folder.
	RUN Commands you want to run on building your image
	EXPOSE your ports you can list many port separated by space
	CMD Command you want to be run when run you image.
	VOLUME Volume path


Example

	FROM debian:stretch-slim
	# all images must have a FROM
	# usually from a minimal Linux distribution like debian or (even better) alpine
	# if you truly want to start with an empty container, use FROM scratch

	ENV NGINX_VERSION 1.13.6-1~stretch
	ENV NJS_VERSION   1.13.6.0.1.14-1~stretch
	# optional environment variable that's used in later lines and set as envvar when container is running

	# you can write many variables with the \ key
	
	ENV SENDER_EMAIL=user@outlook.com \
		SENDER_EMAIL_PASS=pass \
		NODE_ENV=production
	RUN apt-get update \
		&& apt-get install --no-install-recommends --no-install-suggests -y gnupg1 \
		&& \
		NGINX_GPGKEY=573BFD6B3D8FBC641079A6ABABF5BD827BD9BF62; \
		found=''; \
		for server in \
			ha.pool.sks-keyservers.net \
			hkp://keyserver.ubuntu.com:80 \
			hkp://p80.pool.sks-keyservers.net:80 \
			pgp.mit.edu \
		; do \
			echo "Fetching GPG key $NGINX_GPGKEY from $server"; \
			apt-key adv --keyserver "$server" --keyserver-options timeout=10 --recv-keys "$NGINX_GPGKEY" && found=yes && break; \
		done; \
		test -z "$found" && echo >&2 "error: failed to fetch GPG key $NGINX_GPGKEY" && exit 1; \
		apt-get remove --purge -y gnupg1 && apt-get -y --purge autoremove && rm -rf /var/lib/apt/lists/* \
		&& echo "deb http://nginx.org/packages/mainline/debian/ stretch nginx" >> /etc/apt/sources.list \
		&& apt-get update \
		&& apt-get install --no-install-recommends --no-install-suggests -y \
							nginx=${NGINX_VERSION} \
							nginx-module-xslt=${NGINX_VERSION} \
							nginx-module-geoip=${NGINX_VERSION} \
							nginx-module-image-filter=${NGINX_VERSION} \
							nginx-module-njs=${NJS_VERSION} \
							gettext-base \
		&& rm -rf /var/lib/apt/lists/*
	# optional commands to run at shell inside container at build time
	# this one adds package repo for nginx from nginx.org and installs it

	RUN ln -sf /dev/stdout /var/log/nginx/access.log \
		&& ln -sf /dev/stderr /var/log/nginx/error.log
	# forward request and error logs to docker log collector

	EXPOSE 80 443 8080
	# expose these ports on the docker virtual network
	# you still need to use -p or -P to open/forward these ports on host

	CMD ["nginx", "-g", "daemon off;"]
	# required: run this command when container is launched
	# only one CMD allowed, so if there are multiple, last one wins


EXAMPLE 2: SIMPLE

    # this shows how we can extend/change an existing official image from Docker Hub
    
    FROM nginx:latest
    # highly recommend you always pin versions for anything beyond dev/learn
    
    WORKDIR /usr/share/nginx/html
    # change working directory to root of nginx webhost
    # using WORKDIR is preferred to using 'RUN cd /some/path'
    
    COPY index.html index.html
    
    # I don't have to specify EXPOSE or CMD because they're in my FROM

Build images using dockerfile

    docker image build -t [YOUR_IMAGE_NAME] [DOCKERFILE LOCATION]
    docker image build -t my-image .


## Volumes
