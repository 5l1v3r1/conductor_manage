# -*- coding: cp1254 -*-
import paramiko
import time
import xmlrpclib
from datetime import datetime


cndt_param = {"authenticationUser":"admin",
                   "authenticationPassword":"PassW0rd",
                   "conferenceName":""}
Conductor = xmlrpclib.ServerProxy("http://10.10.10.10/RPC2")

ssh = paramiko.SSHClient()
ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())
ssh.connect(hostname='10.10.10.10',
            username='admin', password='PassW0rd')

ssh_shell = ssh.invoke_shell()

room='xStatus Conferencefactoryprimaryconferences {} McuConferenceId\n'
participant='xStatus Conferencefactoryprimaryconferences {} UsedAdhocParticipants\n'

class RoomClose():

    def __init__(self, room_number):
        self.room_number = room_number
        ssh_shell.send(participant.format(room_number))
        time.sleep(.5)
        self.output1 = ssh_shell.recv(2048)
        self.room1 = self.output1[self.output1.find('UsedAdhocParticipants:')+23:self.output1.find('*s/')+0]

        ssh_shell.send(room.format(room_number))
        time.sleep(.5)
        self.output2 = ssh_shell.recv(2048)
        self.room_name = self.output2[self.output2.find('"')+1:self.output2.find('"')+10]

    def process(self):
        if int(self.room1) == 1 or 0:
            cndt_param['conferenceName'] = "{}".format(self.room_name)
            cndt_param1 = cndt_param
            Conductor.conference.destroy(cndt_param1)
            print "\n {} room closed...\n".format(self.room_name)

meter = 0
while meter < 13:
    start = datetime.strftime(datetime.now(), '%c')
    meter += 1
    instance = RoomClose(meter)
    try:
        instance.process()
    except ValueError:
        pass
    if meter == 12:
        end = datetime.strftime(datetime.now(), '%c')
        print "Start time {}, End time {}".format(start, end), "\n"
        time.sleep(300)
        print "Start again...".center(50, '*'), "\n"
        meter = 0
