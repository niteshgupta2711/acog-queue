# acog-queue


wriye a queue system using sqlite3 in python with producer ,consumer and a manager to take care of failed tasks pushed to sqlite3 end to end



import sqlite3

#Connect to the database
conn = sqlite3.connect('queue_system.db')

#Create the table
with conn:
    conn.execute('''CREATE TABLE queue
        (id INTEGER PRIMARY KEY,
        name TEXT,
        timestamp INTEGER)''')

#Function to add a person to the queue
def add_to_queue(name, timestamp):
    conn.execute('INSERT INTO queue (name, timestamp) VALUES (?, ?)', (name, timestamp))
    conn.commit()

#Function to remove a person from the queue
def remove_from_queue(id):
    conn.execute('DELETE FROM queue WHERE id=?', (id,))
    conn.commit()

#Function to get the current queue
def get_queue():
    cursor = conn.execute('SELECT * FROM queue ORDER BY timestamp ASC')
    return cursor.fetchall()

#Producer
add_to_queue('John', 15)
add_to_queue('Jane', 20)
add_to_queue('Jack', 10)

#Consumer
remove_from_queue(3)

#Get the current queue
queue = get_queue()
for person in queue:
    print(person)

#Manager
#Loop through the queue and check for failed tasks
while queue:
    task = queue.pop(0)
    if task[2] < time.time():
        #Task failed, log it and take appropriate action
        print('Task failed: ' + task[1])
        #Take action
        #...
    else:
        #Task successful, log it
        print('Task successful: ' + task[1])
