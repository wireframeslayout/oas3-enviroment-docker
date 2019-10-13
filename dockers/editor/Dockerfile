FROM node:8-alpine
ENV PORT 8000
RUN npm install -g swagger-editor-live@2.1.7
CMD swagger-editor-live /api/${APINAME:-api}.yaml --host=0.0.0.0 --port=$PORT