FROM alpine
ADD apisprout-v1.3.0-linux.tar.xz /usr/bin
CMD apisprout /api/${APINAME:-api}.yaml -p$PORT ${WATCH:+-w} ${VALIDATE_REQUEST:+--validate-request} 