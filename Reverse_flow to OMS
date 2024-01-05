RTO_COMPLETED STATES SCRIPT
import requests
import json
import time
import varadhi_auth_headers
import uuid
import csv
from datetime import datetime

OUTPUT_CSV = "/home/jaya.meesal.vc/output.csv"

class MyDict(dict):
def __str__(self):
def encode_utf8(obj):
if isinstance(obj, unicode):  # noqa: F821
return obj.encode('utf-8')
return obj

return json.dumps(self, default=encode_utf8)

def write_to_csv(data, output_csv):
with open(output_csv, 'a') as f:
writer = csv.writer(f)
writer.writerow(data)

def push_to_topic(base_url, topic):
push_url = "{}/topics/{}/messages".format(base_url, topic)
data_path = "/home/jaya.meesal.vc/pickup_completed/shipment_rto_completed_new.csv"

with open("/home/jaya.meesal.vc/pickup_completed/rto_completed_oms.csv", 'r') as rts, open(data_path, 'w') as output_file:
output_writer = csv.writer(output_file)

for line in csv.reader(rts, delimiter=','):
vendor_tracking_id = line[0].strip()
api_url = "http://10.24.1.19/shipments/get_all_shipment_details?vendor_tracking_id={}".format(
vendor_tracking_id)
r2d2_url = "http://10.24.1.19/shipments/vendor_tracking_id/{}".format(vendor_tracking_id)

try:
response = requests.get(api_url)
api_data = response.json()
response2 = requests.get(r2d2_url)
vendor_data = response2.json()

shipment = api_data[0]['shipment']
shipment_data = api_data[0]['seller']
courier_name = vendor_data["vendor_name"]

attributes = [
shipment["merchant_reference_id"],
shipment["vendor_tracking_id"],
shipment["payment_type"],
shipment["merchant_code"],
shipment_data["seller_reference_id"]
]

vendor_id = shipment["vendor_id"]

output_writer.writerow(attributes)

except Exception as ex:
print("Error fetching details for tracking ID {}: {}".format(vendor_tracking_id, ex))
continue

with open(data_path, 'r') as rts:
count = 0
a = 0
varadhi_auth_token = varadhi_auth_headers.get_auth_header()
print(varadhi_auth_token)
for line in csv.reader(rts, delimiter=','):
count = count + 1
headers = varadhi_auth_token
print(headers)
headers["X_TENANT_ID"] = line[3].strip()
headers["X_MERCHANT_CODE"] = line[3].strip()
headers["X_EVENT_NAME"] = "shipment_rto_completed"
headers["X_TOPIC_NAME"] = "production.b2c.fkl.ship.status"
headers["X_MOVEMENT_TYPE"] = "OutgoingShipment"
try:
merchant_reference_id, vendor_tracking_id, payment_type, merchant_code, seller_reference_id = map(str.strip, line)
vendor_id = shipment["vendor_id"]
courier_name = vendor_data["vendor_name"]

current_uuid = str(uuid.uuid4())
headers["X_RESTBUS_MESSAGE_ID"] = current_uuid
headers["X_RESTBUS_GROUP_ID"] = current_uuid
headers["X_RESTBUS_TRANSACTION_ID"] = current_uuid

data = {
source: "FKL",
entity_id: merchant_reference_id,
merchant_code: merchant_code,
payment_type: payment_type,
vendor_tracking_id: vendor_tracking_id,
shipment_type: "OutgoingShipment",
vendor_id: vendor_id,
merchant_reference_id: merchant_reference_id,
event: "shipment_rto_completed",
event_date: datetime.now().isoformat(),
seller_id: seller_reference_id,
courier_name: courier_name
}

headers = MyDict(headers)
print("headers:", headers)
print("data:", data)
print("url:", push_url)

try:
r = requests.post(push_url, json=data, headers=headers)
print(r.status_code)
csv_data = [merchant_reference_id, str(r.status_code)]
write_to_csv(csv_data, OUTPUT_CSV)
except Exception as ex:
time.sleep(5)
csv_data = [merchant_reference_id, str(ex)]
write_to_csv(csv_data, OUTPUT_CSV)
print("bhai bhai bhai......fataaaaaaaaaaaaa", ex)

except Exception as bhai:
print("Bhai kar kya raha hai !!!!!", bhai)
csv_data = [merchant_reference_id, str(bhai)]
write_to_csv(csv_data, OUTPUT_CSV)
continue

if __name__ == "__main__":
base_url = "http://10.24.0.208"
topic = "production.b2c.fkl.ship.status"
push_to_topic(base_url, topic)
print("done dona done done..")
OUT_FOR_DELIVERY  STATES SCRIPT



import requests
import json
import time
import varadhi_auth_headers
import uuid
import csv
from datetime import datetime

OUTPUT_CSV = "/home/jaya.meesal.vc/output.csv"

class MyDict(dict):
def __str__(self):
def encode_utf8(obj):
if isinstance(obj, unicode):  # noqa: F821
return obj.encode('utf-8')
return obj

return json.dumps(self, default=encode_utf8)

def write_to_csv(data, output_csv):
with open(output_csv, 'a') as f:
writer = csv.writer(f)
writer.writerow(data)

def push_to_topic(base_url, topic):
push_url = "{}/topics/{}/messages".format(base_url, topic)
data_path = "/home/jaya.meesal.vc/pickup_completed/shipment_received_new.csv"

with open("/home/jaya.meesal.vc/pickup_completed/received_oms.csv", 'r') as rts, open(data_path, 'w') as output_file:
output_writer = csv.writer(output_file)

for line in csv.reader(rts, delimiter=','):
vendor_tracking_id = line[0].strip()
api_url = "http://10.24.1.19/shipments/get_all_shipment_details?vendor_tracking_id={}".format(
vendor_tracking_id)
r2d2_url = "http://10.24.1.19/shipments/vendor_tracking_id/{}".format(vendor_tracking_id)

try:
response = requests.get(api_url)
api_data = response.json()
response2 = requests.get(r2d2_url)
vendor_data = response2.json()

shipment = api_data[0]['shipment']
shipment_data = api_data[0]['seller']
courier_name = vendor_data["vendor_name"]

attributes = [
shipment["merchant_reference_id"],
shipment["vendor_tracking_id"],
shipment["payment_type"],
shipment["merchant_code"],
shipment_data["seller_reference_id"]
]

vendor_id = shipment["vendor_id"]

output_writer.writerow(attributes)

except Exception as ex:
print("Error fetching details for tracking ID {}: {}".format(vendor_tracking_id, ex))
continue

with open(data_path, 'r') as rts:
count = 0
a = 0
varadhi_auth_token = varadhi_auth_headers.get_auth_header()
print(varadhi_auth_token)
for line in csv.reader(rts, delimiter=','):
count = count + 1
headers = varadhi_auth_token
print(headers)
headers["X_TENANT_ID"] = line[3].strip()
headers["X_MERCHANT_CODE"] = line[3].strip()
headers["X_EVENT_NAME"] = "shipment_received"
headers["X_TOPIC_NAME"] = "production.b2c.fkl.ship.status"
headers["X_MOVEMENT_TYPE"] = "OutgoingShipment"
try:
merchant_reference_id, vendor_tracking_id, payment_type, merchant_code, seller_reference_id = map(str.strip, line)
vendor_id = shipment["vendor_id"]
courier_name = vendor_data["vendor_name"]

current_uuid = str(uuid.uuid4())
headers["X_RESTBUS_MESSAGE_ID"] = current_uuid
headers["X_RESTBUS_GROUP_ID"] = current_uuid
headers["X_RESTBUS_TRANSACTION_ID"] = current_uuid

data = {
source: "FKL",
entity_id: merchant_reference_id,
merchant_code: merchant_code,
payment_type: payment_type,
vendor_tracking_id: vendor_tracking_id,
shipment_type: "OutgoingShipment",
vendor_id: vendor_id,
merchant_reference_id: merchant_reference_id,
event: "shipment_received",
event_date: datetime.now().isoformat(),
seller_id: seller_reference_id,
courier_name: courier_name
}

headers = MyDict(headers)
print("headers:", headers)
print("data:", data)
print("url:", push_url)

try:
r = requests.post(push_url, json=data, headers=headers)
print(r.status_code)
csv_data = [merchant_reference_id, str(r.status_code)]
write_to_csv(csv_data, OUTPUT_CSV)
except Exception as ex:
time.sleep(5)
csv_data = [merchant_reference_id, str(ex)]
write_to_csv(csv_data, OUTPUT_CSV)
print("bhai bhai bhai......fataaaaaaaaaaaaa", ex)

except Exception as bhai:
print("Bhai kar kya raha hai !!!!!", bhai)
csv_data = [merchant_reference_id, str(bhai)]
write_to_csv(csv_data, OUTPUT_CSV)
continue

if __name__ == "__main__":
base_url = "http://10.24.0.208"
topic = "production.b2c.fkl.ship.status"
push_to_topic(base_url, topic)
print("done dona done done..")
