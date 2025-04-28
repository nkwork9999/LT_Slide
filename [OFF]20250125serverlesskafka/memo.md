AWS Lambda Kafka Hands-on

# Serverless data processing

Shimokawa Kensuke

Amazon CTO warner?

- Simplexy
  Intented Complexy

# msk

lambda
appsync:managed graqh ql

Unicorns 移動
RestAPI Overfetch

Mutation:DynamoDB に push Update?

- EventAPI:Pub/Sub APIs serverless WebSocket Subscribe API Client Real-Time お互いがコミュニケーション

- MSK Serverless
  Kafka: サービスの Bus REST でアクセス可能 データストア → ストリーミング

  Kafka topic = kinesis stream
  キューとの違い　データストアである　キューだと読み込まれると出ていってしまう。 動画ザッピング
  Kafka オフセット Kinesis シーケンス

  EventBridge Pipess API Destination 　パイプライン

  Kafka sink connector で Lambda に繋げる

  Lambda ポーリングなし　 EventPattern 内

#配信セマンティクス
At-least once 配信
何回でも結果が変わらないように
SQS

Exactly-once 配信
一回だけでよい
Kinesis
SNS FIFO
SWS FIFO
Kafka Topic

-順序セマンティクス
順序保証なし
SQS
SNS
EventBridge

順序保証あり
Kinesis
SNS FIFO トピック
SQS FIFO キュー
Kafka Topic

# もくもく

MskClusterARN
arn:aws:kafka:us-east-1:440208447096:cluster/MSKCluster-workshop/39c2bde2-11ae-47a7-90db-4a09b130890f-s1

Password
KhAGnWg3vAOsJ54C

UnicornAppApiEndpoint
https://3pzneg2cszaytmbkvi5ojo7jom.appsync-api.us-east-1.amazonaws.com/graphql

UnicornAppApiKey
da2-sumit7dsfjh7thh577yjnihd4y

URL
https://d3q2ru9ekhd1vh.cloudfront.net/?folder=/Workshop

only require setting up the variables to connect to the right MSK cluster.

import json
import os
import requests
import base64

API_CHANNEL = os.environ['API_CHANNEL']
API_URL = os.environ['API_URL']
API_KEY = os.environ['API_KEY']

session = requests.Session()

def lambda_handler(event, context):

for name, messages in event["records"].items():

      for message in messages:
            query = 'mutation PublishData {{ publish(data: "{}", name: "{}") {{ data name }} }}'
            decoded_message = base64.b64decode(message["value"]).decode('utf-8')
            payload = {"msg": decoded_message.replace('"',"'")}

            merged_query = query.format(json.dumps(payload).replace('"','\\"'), API_CHANNEL)
            response = session.request( url=API_URL, method='POST', headers={'x-api-key': API_KEY}  ,json={'query': merged_query})
            print(response.json())

return {
'statusCode': 200,
'name': 'message relay',
'body': json.dumps({"msg":"unicorn telemetry updates!"})
}

export MSK_CLUSTER_ARN=$(aws cloudformation describe-stacks --query 'Stacks[?StackName==`workshop`][].Outputs[?OutputKey==`MskClusterARN`].OutputValue' --output text)
export MSK_BROKER=$(aws kafka get-bootstrap-brokers --cluster-arn $MSK_CLUSTER_ARN | jq .BootstrapBrokerStringSaslIam -r)
export MSK_TOPIC=unicorns

echo $MSK_CLUSTER_ARN
echo $MSK_BROKER
echo $MSK_TOPIC

/Workshop/code/producer-msk

import { Amplify, API, graphqlOperation } from "aws-amplify";

import awsconfig from "./aws-exports";

Amplify.configure(awsconfig);

("use strict");

(function () {
const Dashboard = {};

Dashboard.Map = function (L) {
const circleColor = "blue";
const circleRadius = 50;
const gcFrequency = 1000;
const iconSize = 30;
const iconUrl = "images/unicorn-icon.png";
const lineColor = "orange";
const mapAttribution =
'Map data &copy; <a href="http://openstreetmap.org">OpenStreetMap</a> contributors, <a href="http://creativecommons.org/licenses/by-sa/2.0/">CC-BY-SA</a>, Imagery © <a href="http://mapbox.com">Mapbox</a>';
const mapBoxAccessToken =
"pk.eyJ1IjoicGlnbmF0YWoiLCJhIjoiY2o3a2xrcDlxMHBxeTJxcW5wa2JlZW1mbyJ9.7dKS6nM8xwV-92s_3HMWtA";
const mapBoxEndpoint =
"https://api.mapbox.com/styles/v1/mapbox/{id}/tiles/{z}/{x}/{y}?access_token={accessToken}";
const mapBoxLayerId = "streets-v11";
const mapCenter = [40.749167, -73.985184];
const mapZoom = 12;
const unicornTimeout = 30000;
const metersPerSecondInMPH = 2.23694;

    return {
      init: function (divName) {
        this.icon = L.icon({
          iconSize: iconSize,
          iconUrl: iconUrl,
        });
        this.map = L.map(divName).setView(mapCenter, mapZoom);
        this.unicorns = {};

        L.circle(mapCenter, {
          color: circleColor,
          radius: circleRadius,
        }).addTo(this.map);

        garbageCollector(this.unicorns, this.map);

        return this;
      },

      render: function () {
        L.tileLayer(mapBoxEndpoint, {
          accessToken: mapBoxAccessToken,
          attribution: mapAttribution,
          id: mapBoxLayerId,
        }).addTo(this.map);

        return this;
      },

      update: function (status) {
        if (!this.unicorns.hasOwnProperty(status.Name)) this.add(status);

        const unicorn = this.unicorns[status.Name];
        const mph = (status.Distance * metersPerSecondInMPH).toFixed(1);
        const popUpMsg = `
          <p id="popUpName">${status.Name}</p>

          <table id="popUpDetails">
            <tr>
              <td class="popUpDetailsHeader">Speed</td>
              <td>${mph} mph</td>
            </tr>
            <tr>
              <td class="popUpDetailsHeader">Health</td>
              <td>${status.HealthPoints} points</td>
            </tr>
            <tr>
              <td class="popUpDetailsHeader">Magic</td>
              <td>${status.MagicPoints} points</td>
            </tr>
          </table>
        `;

        unicorn.polyline.addLatLng([status.Latitude, status.Longitude]);
        unicorn.marker
          .setLatLng([status.Latitude, status.Longitude])
          .bindPopup(popUpMsg);

        unicorn.lastStatusTime = new Date();

        return this;
      },

      add: function (status) {
        const unicorn = {};

        unicorn.polyline = L.polyline([mapCenter], {
          color: lineColor,
        }).addTo(this.map);
        unicorn.marker = L.marker([status.Latitude, status.Longitude], {
          icon: this.icon,
        }).addTo(this.map);

        this.unicorns[status.Name] = unicorn;

        return this;
      },
    };

    function garbageCollector(unicorns, map) {
      setInterval(function () {
        Object.keys(unicorns).forEach(function (unicornName) {
          const unicorn = unicorns[unicornName];
          const now = new Date().getTime();

          if (now - unicorn.lastStatusTime.getTime() > unicornTimeout) {
            unicorn.polyline.removeFrom(map);
            unicorn.marker.removeFrom(map);

            delete unicorns[unicornName];
          }
        });
      }, gcFrequency);
    }

};

const onPublish = /_ GraphQL _/ `  subscription SubscribeToStream {
      subscribe(name: "unicorns") {
        name
        data
      }
    }`;

$(window).on("load", function () {
const map = Dashboard.Map(L).init("map").render();

    $("form").on("submit", function (e) {
      e.preventDefault();
    });

    $("#modal").modal("show");

    API.graphql(graphqlOperation(onPublish)).subscribe({
      next: (evt) => {
        const msg = JSON.parse(evt.value.data.subscribe.data);
        const status = msg.msg.replace(/'/g, '"');
        console.log(status);
        map.update(JSON.parse(status));
      },
    });

});
})();

# LT

##

IAM Identity Center
可視化
新しいサービスがリージョン限定

外部 IdP?

useast リージョン

## abetaku

conpass フィード Discord イベント登録

##

Agent for Amazon Bedrock

実行計画　タスクを実行
CloudWatch Logs ログ収集 インスタンス再起動

##

スタッフエンジニア
と同等
マネージャー
部下がいない
