# maccchanger
import subprocess
import optparse
import re

print(""""


███╗░░░███╗░█████╗░░█████╗░        ░█████╗░██╗░░██╗░█████╗░███╗░░██╗░██████╗░███████╗██████╗░
████╗░████║██╔══██╗██╔══██╗        ██╔══██╗██║░░██║██╔══██╗████╗░██║██╔════╝░██╔════╝██╔══██╗
██╔████╔██║███████║██║░░╚═         ██║░░╚═╝███████║███████║██╔██╗██║██║░░██╗░█████╗░░██████╔╝
██║╚██╔╝██║██╔══██║██║░░██╗        ██║░░██╗██╔══██║██╔══██║██║╚████║██║░░╚██╗██╔══╝░░██╔══██╗
██║░╚═╝░██║██║░░██║╚█████╔╝        █████╔╝██║░░██║██║░░██║██║░╚███║╚██████╔╝███████╗██║░░██║
╚═╝░░░░░╚═╝╚═╝░░╚═╝░╚════╝░         ╚════╝░╚═╝░░╚═╝╚═╝░░╚═╝╚═╝░░╚══╝░╚═════╝░╚══════╝╚═╝░░╚═╝

""")
def get_arguments():
    parser = optparse.OptionParser()
    parser.add_option('-i', "--interface", dest="interface", help="add Interface to change mac address")
    parser.add_option('-m', "--mac", dest="new_mac", help=" new mac address to use")
    (options, argument) = parser.parse_args()
    if not options.interface:
        parser.error("[-]Please specify an interface, use --help for more info")
    elif not options.new_mac:
        parser.error("[-]Please specify a new mac address, use --help for more info")
    return options

def change_mac(interface,new_mac):
    print("[+]Changing mac to " + new_mac)

    subprocess.call(["ifconfig", interface, "down"])
    subprocess.call(["ifconfig", interface, "hw", "ether", new_mac])
    subprocess.call(["ifconfig", interface, "up"])

def get_current_mC(interface):
    ifconfig_result=subprocess.check_output(["ifconfig",options.interface])


    mac_adress_search_result= re.search(r"\w\w:\w\w:\w\w:\w\w:\w\w:\w\w",ifconfig_result)
    
    if mac_adress_search_result:
        return mac_adress_search_result.group(0)
    else:
        print("[-] could not read MAC address") 
    

options=get_arguments()
curr_mac=get_current_mC(options.interface, options.new_mac)
print("Current Mac="+str(curr_mac))
change_mac(options.interface,options.new_mac)

if curr_mac==options.new_mac:
    print("[+] MAC address was successfully changed to "+ curr_mac)
else:
    print("[-] MAC address did not get changed")
