FROM golang:1.10-alpine as builder
RUN apk add --no-cache ca-certificates git && \
      wget -qO/go/bin/dep https://github.com/golang/dep/releases/download/v0.5.0/dep-linux-amd64 && \
      chmod +x /go/bin/dep

ENV PROJECT github.com/shahadarsh/docker-k8s-workshop/exercises/docker/conference-website

WORKDIR /go/src/$PROJECT

# restore dependencies
COPY Gopkg.* ./
RUN dep ensure --vendor-only -v
COPY . .
RUN go install .

FROM alpine as release
RUN apk add --no-cache ca-certificates \
    busybox-extras net-tools bind-tools
WORKDIR /conference-website
COPY --from=builder /go/bin/conference-website /conference-website/server
COPY ./templates ./templates

EXPOSE 8080
ENTRYPOINT ["/conference-website/server"]
