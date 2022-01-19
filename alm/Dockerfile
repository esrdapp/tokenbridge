FROM node:12 as contracts

WORKDIR /tokenbridge

COPY contracts/package.json contracts/package-lock.json ./contracts/

WORKDIR /tokenbridge/contracts
RUN npm install --only=prod

COPY ./contracts/truffle-config.js ./
COPY ./contracts/contracts ./contracts
RUN npm run compile

FROM node:12 as alm-builder

WORKDIR /tokenbridge
COPY package.json .
COPY --from=contracts /tokenbridge/contracts/build ./contracts/build
COPY commons/package.json ./commons/
COPY alm/package.json ./alm/
COPY yarn.lock .
#RUN NOYARNPOSTINSTALL=1 yarn install --frozen-lockfile

COPY ./commons ./commons
COPY ./alm ./alm

ARG DOT_ENV_PATH=./alm/.env
COPY ${DOT_ENV_PATH} ./alm/.env

WORKDIR /tokenbridge/alm
RUN yarn run build


FROM node:12 as alm-production
RUN yarn global add serve
WORKDIR /app
COPY --from=alm-builder /tokenbridge/alm/build .
CMD serve -p $PORT -s .
