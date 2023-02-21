targets:=recv_csi send_data send_Data_con

all: $(targets)

recv_csi: csi_fun.o main.o
	$(CC) $^ -o $@

csi_fun.o: csi_fun.c csi_fun.h
	$(CC) -c $< -o $@

main.o: main.c csi_fun.h
	$(CC) -c $< -o $@

send_data: sendData.c
	$(CC) $^ -o $@

send_Data_con: sendData_con.c
	$(CC) $^ -o $@

clean:
	rm -f *.o $(targets)

