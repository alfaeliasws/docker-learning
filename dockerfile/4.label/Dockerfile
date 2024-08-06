FROM alpine:3

LABEL author="Alfa"
LABEL company="Al-Solution" website="https://alfaeliasws.netlify.app"

# Executed in building image
RUN mkdir hello
RUN echo "Hello World" > "hello/world.txt"

# Executed in container run
CMD cat "hello/world.txt"