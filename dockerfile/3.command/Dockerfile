FROM alpine:3

# Executed in building image
RUN mkdir hello
RUN echo "Hello World" > "hello/world.txt"

# Executed in container run
CMD cat "hello/world.txt"