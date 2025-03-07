FROM nginx@sha256:af296b188c7b7df99ba960ca614439c99cb7cf252ed7bbc23e90cfda59092305

# can be passed during Docker build as build time environment for github branch to pickup configuration from.
ARG container_user=mosip

# can be passed during Docker build as build time environment for github branch to pickup configuration from.
ARG container_user_group=mosip

# can be passed during Docker build as build time environment for github branch to pickup configuration from.
ARG container_user_uid=1001

# can be passed during Docker build as build time environment for github branch to pickup configuration from.
ARG container_user_gid=1001

# install packages and create user
RUN apt-get -y update &&\
apt-get install -y --no-install-recommends unzip wget zip tree openjdk-11-jdk &&\
groupadd -g ${container_user_gid} ${container_user_group} &&\
rm -rf /var/lib/apt/lists/* &&\
wget https://dlcdn.apache.org/maven/maven-3/3.8.8/binaries/apache-maven-3.8.8-bin.tar.gz &&\
tar -xzf apache-maven-3.8.8-bin.tar.gz && mv apache-maven-3.8.8 /usr/local/maven && rm apache-maven-3.8.8-bin.tar.gz &&\
useradd -u ${container_user_uid} -g ${container_user_group} -s /bin/sh -m ${container_user} \
&& mkdir -p /var/run/nginx /var/tmp/nginx \
&& chown -R ${container_user}:${container_user} /usr/share/nginx /var/run/nginx /var/tmp/nginx

COPY ./nginx.conf /etc/nginx/nginx.conf

ENV MAVEN_HOME /usr/local/maven
ENV PATH $MAVEN_HOME/bin:$PATH
ENV JAVA_HOME /usr/lib/jvm/java-11-openjdk-amd64

# set working directory for the user
WORKDIR /home/${container_user}

ENV work_dir=/home/${container_user}

# environment variable for basepath
ENV base_path=/usr/share/nginx/html/artifactory

# environment variable for Biosdk client Zip path
ENV biosdk_client_zip_path=${base_path}/libs-release-local/biosdk/mock/0.9

# environment variable for Demosdk Zip path
ENV demosdk_zip_path=${base_path}/libs-release-local/demosdk/default/

# environment variable for Biosdk lib Zip path
ENV biosdk_lib_zip_path=${base_path}/libs-release-local/biosdk/

# environment variable for Biosdk lib Zip path
ENV jpegsdk_lib_zip_path=${base_path}/libs-release-local/jpeg-sdk/

# environment variable for Cache Provider Jar path
ENV cache_path=${base_path}/libs-release-local/cache

# environment variable for ida shell scripts path
ENV ida_sh_path=${base_path}/libs-release-local/deployment/docker/id-authentication

# environment variable for hsm client path
ENV hsm_client_path=${base_path}/libs-release-local/hsm

# environment variable for cloudhsm client path
ENV cloudhsm_client_path=${base_path}/libs-release-local/cloudhsm

# environment variable for mosip-plugins zip path
ENV mosip_plugins_zip_path=${base_path}/libs-release-local/mosip-plugins

# environment valiable for sdk jar
ENV sdk_path=${base_path}/libs-release-local/mock-sdk/1.1.5/

# environment valiable for sdk jar
ENV jpeg_sdk_path=${base_path}/libs-release-local/jpeg-mock-sdk/1.1.5/

# environment variable for Kernel Jars
ENV kernel_jar_path=${base_path}/libs-release-local/io/mosip/kernel

# environment variable for regproc reprocessor ceylon cache jar path
ENV test_jar_path=${base_path}/libs-release-local/io/mosip/testing/

# environment variable for ref-idobjectvalidator jar
ENV idobject_jar_path=${base_path}/libs-release-local/io/mosip/kernel/kernel-ref-idobjectvalidator

# environment variable for regproc auth jar
ENV regproc_jar_path=${base_path}/libs-release-local/io/mosip/kernel/kernel-auth-adapter

# environment variable for prereg transliteration jar path
ENV icu4j_jar_path=${base_path}/libs-release-local/icu4j

# environment variable for i18n zip path
ENV i18n_zip_path=${base_path}/libs-release-local/i18n

# environment variable for idp auth wrapper zip path
ENV idp_auth_wrapper_lib_zip_path=${base_path}/libs-release-local/idp/idp-auth-wrapper

# environment variable for esignet wrappers zip path
ENV esignet_wrapper_lib_zip_path=${base_path}/libs-release-local/esignet

# environment variable for Clamav dependency
ENV clamav_path=${base_path}/libs-release-local/clamav

ENV regclient_jar_path=${base_path}/libs-release-local

ENV master_template_path=${base_path}/libs-release-local/master-templates

# environment variable for child auth filter jar path
ENV child_auth_filter_jar_path=${base_path}/libs-release-local/io/mosip/authentication/authentication-ref-impl

# environment variable for script path
ENV scripts_path=/home/mosip/scripts

# environment variable for jar version to be downloaed, this needed to be passed buildtime
ENV version=1.2.1-SNAPSHOT
ENV idp_auth_wrapper_version=0.0.1-SNAPSHOT

# Create all the jar paths.
RUN mkdir -p ${biosdk_client_zip_path}/biosdk-client ${demosdk_zip_path}/demosdk ${biosdk_lib_zip_path}/biosdk-lib ${cache_path} ${ida_sh_path} ${hsm_client_path} ${mosip_plugins_zip_path} ${kernel_jar_path} ${test_jar_path} ${idobject_jar_path} ${regproc_jar_path} ${sdk_path} ${icu4j_jar_path} ${clamav_path} ${i18n_zip_path} ${child_auth_filter_jar_path} ${base_path}/libs-release-local/reg-client ${scripts_path} ${master_template_path} ${jpeg_sdk_path} ${jpegsdk_lib_zip_path}/jpeg-sdk-lib/ ${idp_auth_wrapper_lib_zip_path} ${esignet_wrapper_lib_zip_path}/esignet-wrapper

# Copy all the respective jars to the location

COPY /src/sdk/biosdk/install.sh ${biosdk_client_zip_path}/biosdk-client/

COPY /src/sdk/biosdk/biosdk-client* ${biosdk_client_zip_path}/biosdk-client/

COPY /src/sdk/demosdk/install.sh ${demosdk_zip_path}/demosdk/

COPY /src/deployment/docker/id-authentication/* ${ida_sh_path}/

COPY /src/cache/* ${cache_path}/

COPY /src/hsm/client.zip ${hsm_client_path}/

COPY /src/cloudhsm/* ${cloudhsm_client_path}/

COPY /src/mosip-plugins/sign-in-with-esignet/sign-in-with-esignet.zip ${mosip_plugins_zip_path}/

COPY /src/testing/regproc-reprocessor-ceylon-cache-repo.zip ${test_jar_path}/

COPY /src/testing/glowroot.zip ${test_jar_path}/

COPY /src/i18n/regclient-i18n-bundle/*  ${work_dir}/resources/

COPY /src/i18n/admin-entity-spec-bundle/*  ${work_dir}/admin-entity-spec-bundle/

COPY /src/i18n/admin-i18n-bundle/*  ${work_dir}/admin-i18n-bundle/

COPY /src/i18n/pmp-entity-spec-bundle/*  ${work_dir}/pmp-entity-spec-bundle/

COPY /src/i18n/pmp-i18n-bundle/*  ${work_dir}/pmp-i18n-bundle/

COPY /src/i18n/pre-registration-i18n-bundle/*  ${work_dir}/pre-registration-i18n-bundle/

COPY /src/master-templates/* ${work_dir}/master-templates/

COPY /src/i18n/idp-i18n-bundle/*  ${work_dir}/idp-i18n-bundle/

COPY /src/i18n/oidc-demo-i18n-bundle/*  ${work_dir}/oidc-demo-i18n-bundle/

COPY /src/i18n/mock-relying-party-i18n-bundle/*  ${work_dir}/mock-relying-party-i18n-bundle/

COPY /src/i18n/esignet-i18n-bundle/*  ${work_dir}/esignet-i18n-bundle/

COPY /src/jre/zulu11.41.23-ca-fx-jre11.0.8-win_x64.zip ${regclient_jar_path}/zulu11.41.23-ca-fx-jre11.0.8-win_x64.zip

COPY /src/icu4j/* ${icu4j_jar_path}/

COPY /src/auth/* ${regproc_jar_path}/

COPY ./configure.sh ${work_dir}/

COPY pom.xml ${work_dir}/

RUN cd ${work_dir} && mvn dependency:resolve && mvn clean install

RUN bash configure.sh

RUN sed -i -e '/listen/!b' -e '/80;/!b' -e 's/80;/8080;/' /etc/nginx/conf.d/default.conf

# select container user for all tasks
USER ${container_user_uid}:${container_user_gid}

EXPOSE 8080

CMD ["nginx", "-g", "daemon off;"]
