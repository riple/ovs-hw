SIM_TOP=tb_system.v
SOURCES=$(wildcard ../rtl/system.v) \
	$(wildcard ../../../cores/asfifo/rtl/*.v) \
	$(wildcard ../../../cores/sfifo/rtl/*.v) \
	$(wildcard ../../../cores/fifo9togmii/rtl/*.v) \
	$(wildcard ../../../cores/gmii2fifo9/rtl/*.v) \
	$(wildcard ../../../cores/lookupflow/rtl/*.v) \
	$(wildcard ../../../cores/cmdrecv/rtl/*.v) \
	$(wildcard ../../../cores/forwarder/rtl/*.v) \
	$(wildcard ../../../cores/sram/rtl/*.v) \
	$(wildcard ../../../cores/dpram/rtl/*.v) \
	$(wildcard ../../../cores/nic/rtl/*.v) \
	$(wildcard ../../../cores/arp/rtl/*.v) \
	$(wildcard ../../../cores/mixer/rtl/*.v) 

all: phy_test.hex isim

gtk: all
	gtkwave test.vcd

isim: tb_system
	./tb_system

phy_test.hex: phy_ping.hex phy_telnet.hex
	sed "s/#.*$$//g" < phy_ping.hex | sed s/^/1_/g > tmp0 
	sed "s/#.*$$//g" < phy_telnet.hex | sed s/^/1_/g > tmp1 
	cat phy_0.hex tmp0 phy_0.hex tmp1 phy_0.hex > phy_test.hex
	rm tmp0 tmp1

cversim: $(SIM_TOP) $(SOURCES)
	cver $(SOURCES)

lint:
	verilator --lint-only setup.v ${SOURCES}

clean:
	rm -f tb_system verilog.log test.vcd phy_test.hex

tb_system: $(SIM_TOP) $(SOURCES)
	iverilog -o tb_system $(SIM_TOP) $(SOURCES)

.PHONY: clean isim cversim gtk
