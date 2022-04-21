.. code-block::

    import gjavac.lib.*;
    import kotlin.Pair;


    import static gjavac.lib.UvmCoreLibs.*;


    @Component
    public class Utils {
        public String NOT_INITED() {
            return "NOT_INITED";
        }


        public String COMMON() {
            return "COMMON";
        }


        public String PAUSED() {
            return "PAUSED";
        }


        public String STOPPED() {
            return "STOPPED";
        }

        public final long checkInteger(String numstr) {
            if (isBlank(numstr)) {
                error("integer format error of " + numstr);
                return 0;
            }
            return tointeger(numstr);
        }

        public final String getFromAddress() {
            String fromAddress;
            final String prevContractId = get_prev_call_frame_contract_address();
            if (isNotBlank(prevContractId) && is_valid_address(prevContractId)) {
                fromAddress = prevContractId;
            } else {
                fromAddress = caller_address();
            }
            return fromAddress;
        }

        public final void checkAdmin(StableTokenContractDemo self) {
            String fromAddress = getFromAddress();
            if (self.getStorage().admin != fromAddress) {
                error("you are not admin, can't call this function");
            }
        }

        public final void checkMinter(StableTokenContractDemo self) {
            String fromAddress = getFromAddress();
            if (self.getStorage().minter != fromAddress) {
                error("you are not minter, can't call this function");
            }
        }

        public final UvmArray<String> parseArgs(String arg, int count, String errorMsg) {
            if (isBlank(arg)) {
                error(errorMsg);
                return UvmArray.create();
            }
            UvmStringModule stringModule = importModule(UvmStringModule.class, "string");
            UvmArray<String> parsed = stringModule.split(arg, ",");
            if (parsed != null && parsed.size() == count) {
                return parsed;
            } else {
                error(errorMsg);
                return UvmArray.create();
            }
        }

        public final UvmArray<String> parseAtLeastArgs(String arg, int count, String errorMsg) {
            if (isBlank(arg)) {
                error(errorMsg);
                return UvmArray.create();
            }

            UvmStringModule stringModule = importModule(UvmStringModule.class, "string");
            UvmArray<String> parsed = stringModule.split(arg, ",");
            if (parsed != null && parsed.size() >= count) {
                return parsed;
            } else {
                error(errorMsg);
                return UvmArray.create();
            }

        }

        public final boolean arrayContains(UvmArray col, Object item) {
            if (col != null && item != null) {
                ArrayIterator colTter = col.ipairs();
                for (Pair colKeyValuePari = (Pair) colTter.invoke(col, 0);
                    colKeyValuePari.getFirst() != null;
                    colKeyValuePari = (Pair) colTter.invoke(col, colKeyValuePari.getFirst())) {
                    if (colKeyValuePari != null && colKeyValuePari.getSecond() == item) {
                        return true;
                    }
                }
                return false;
            } else {
                return false;
            }
        }

        public final void checkState(StableTokenContractDemo self) {
            String state = self.getStorage().state;
            if (state == NOT_INITED())
                error("contract token not inited");

            if (state == PAUSED())
                error("contract paused");

            if (state == STOPPED())
                error("contract stopped");
        }

        public final void checkStateInited(StableTokenContractDemo self) {
            if (self.getStorage().state == NOT_INITED())
                error("contract token not inited");
        }

        public final boolean checkAddress(String addr) {
            boolean result = is_valid_address(addr);
            if (!result) {
                error("address format error");
            }
            return result;
        }

        public final String getBalanceOfUser(StableTokenContractDemo self, String addr) {
            Object balance = fast_map_get("users", addr);
            if (balance == null) {
                return "0";
            }
            return tostring(balance);
        }

        public final boolean isBlank(String str) {
            return str == null || str.length() == 0;
        }

        public final boolean isNotBlank(String str) {
            return !isBlank(str);
        }
    }
