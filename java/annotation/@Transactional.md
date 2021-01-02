# @Transactional

 

@Transactional(value="transactionManagerEvent", propagation=Propagation.REQUIRED, rollbackFor=Exception.class)