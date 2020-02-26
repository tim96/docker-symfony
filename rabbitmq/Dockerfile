FROM rabbitmq:3.8-management

# To view all plugins
# rabbitmq-plugins list

RUN rabbitmq-plugins enable --offline rabbitmq_mqtt rabbitmq_federation_management rabbitmq_stomp

# Expose ports.
EXPOSE 5672
EXPOSE 15672
