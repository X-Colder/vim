#!/usr/bin/env python
# -*- coding:utf-8 -*-
# Author:pitmaner
# Datetime:2018/2/9 上午9:41
# Software: PyCharm
# Project: tushare_test
# Filename: demo01

import tushare as ts
import tornado.ioloop
import tornado.web
import pymongo
import json
import sys


MONGODB_CONFIG = {
    'host': '127.0.0.1',
    'port': 27017,
    'db_name': 'stock',
    'username': None,
    'password': None
}


class MongoConn(object):

    def __init__(self):
        # connect db
        try:
            self.conn = pymongo.MongoClient(MONGODB_CONFIG['host'], MONGODB_CONFIG['port'])
            self.db = self.conn[MONGODB_CONFIG['db_name']]  # connect db
            self.username = MONGODB_CONFIG['username']
            self.password = MONGODB_CONFIG['password']
            if self.username and self.password:
                self.connected = self.db.authenticate(self.username, self.password)
            else:
                self.connected = True
        except Exception as e:
            print(e)
            print('Connect Statics Database Fail.')
            sys.exit(1)


conn = MongoConn()

def getBasic():
    #conn = pymongo.Connection('127.0.0.1', port=27017)
    conn = MongoConn()
    df = ts.get_stock_basics()
    #print df
    conn.db['baseinfo'].insert(json.loads(df.to_json(orient='table')))


def getReportData():
    df = ts.get_report_data(2017, 4)
    engine = create_engine('mysql://root:dreamer2012@localhost/tushare_test?charset=utf8')
    df.to_sql('reportdata201704', engine, if_exists='append')


def getProfitData():
    df = ts.get_profit_data(2017, 4)
    engine = create_engine('mysql://root:dreamer2012@localhost/tushare_test?charset=utf8')
    df.to_sql('profitdata201704', engine, if_exists='append')


def getGrowthData():
    df = ts.get_growth_data(2017, 4)
    engine = create_engine('mysql://root:dreamer2012@localhost/tushare_test?charset=utf8')
    df.to_sql('growthdata201704', engine, if_exists='append')


def getCashFlow():
    df = ts.get_cashflow_data(2017, 4)
    engine = create_engine('mysql://root:dreamer2012@localhost/tushare_test?charset=utf8')
    df.to_sql('cashflow201704', engine, if_exists='append')


def getDebtPay():
    df = ts.get_debtpaying_data(2017, 4)
    engine = create_engine('mysql://root:dreamer2012@localhost/tushare_test?charset=utf8')
    df.to_sql('debtpay201704', engine, if_exists='append')


def getHs300():
    df = ts.get_hs300s()
    engine = create_engine('mysql://root:dreamer2012@localhost/tushare_test?charset=utf8')
    df.to_sql('hs300', engine, if_exists='append')


def getSz50():
    df = ts.get_sz50s()
    engine = create_engine('mysql://root:dreamer2012@localhost/tushare_test?charset=utf8')
    df.to_sql('sz50', engine, if_exists='append')


def getcodes():
    codes = []
    conn = pymysql.connect("localhost", user='root', password='dreamer2012', db='tushare_test')
    cur = conn.cursor()
    code = cur.execute("select code from basics;")
    for item in cur.fetchall():
        codes.append(item[0])
    return codes


def getnames(code):
    conn = pymysql.connect("localhost", user='root', password='dreamer2012', db='tushare_test')
    cur = conn.cursor()
    name = cur.execute("select name from basics where code = %s;" % code)
    print(cur.fetchall()[0])

def getCode():
    client = pymongo.MongoClient('localhost', 27017)
    db = client['stock']
    coll = db['baseinfo']
    res = coll.find_one()['data']
    codes = []
    for i in range(len(res)):
        code = res[i]['code']
        codes.append(code)
    return codes




def getHisData(code):
    conn = MongoConn()
    df = ts.get_hist_data(code)
    print(df)
    conn.db['hisdata'].insert(json.loads(df.to_json(orient='table')))



def realTimeData():
    df = ts.get_realtime_quotes('002236')
    print(df[['code', 'name', 'price', 'bid', 'ask', 'volume', 'amount', 'time']])
    conn.db['realtime'].insert_many(json.loads(df.to_json(orient='records')))


def hisTickData():
    df = ts.get_tick_data('600516', date='2018-06-29')
    conn.db['tickdata'].insert(json.loads(df.to_json(orient='table')))

class MainHandler(tornado.web.RequestHandler):
    def get(self):
        self.write("Hello, world")


application = tornado.web.Application([
    (r"/", MainHandler),
])






if __name__ == "__main__":
    #application.listen(8888)
    #tornado.ioloop.IOLoop.instance().start()
    # codes = getCode()
    # for code in codes:
    #     print code
    #     getHisData(code)
    #getHisData('603059')
    realTimeData()

        


